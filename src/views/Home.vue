<template lang='pug'>
.page.flex-c
  .full-w.flex-r.bg-white.z2
    table.z3.bg-white
      tr
        th.border.txt-center(v-for='(col,colIndex) of cols.slice(0,fixedColCount)' :key='colIndex' style={width:'60px'}) {{col}}
    .bshd.flex-1.w-1(@touchstart.stop.prevent @touchmove.stop.prevent @touchend.stop.prevent)
      table
        tr
          th(v-for='(col,colIndex) of cols.slice(fixedColCount)' :key='colIndex') {{col}}
  .flex-1.h-1.relative.bsy
    .full-w
      pulldown-loader(:beforePullDown='beforePullDown' :isPullingDown='isPullingDown')
      .full-w.flex-r
        table.z2.bg-white
          tr(v-for='(row,rowIndex) of data' :key='rowIndex')
            td.border.xt-center(v-for='(col,colIndex) of row.slice(0,fixedColCount)' :key='colIndex' style={width:'60px'}) {{col}}
        .bsx.flex-1.w-1
          table
            tr(v-for='(row,rowIndex) of data' :key='rowIndex')
              td(v-for='(col,colIndex) of row.slice(fixedColCount)' :key='colIndex') {{col}}
      pullup-loader(:isPullUpLoad='isPullUpLoad')
</template>

<script>
import BScroll from '@better-scroll/core'
import PullDown from '@better-scroll/pull-down'
import PullUp from '@better-scroll/pull-up'

import PulldownLoader from '@/components/pulldown-loader'
import PullupLoader from '@/components/pullup-loader'

BScroll.use(PullDown)
BScroll.use(PullUp)

const createData = (rowCount, colCount) =>
  new Array(rowCount)
    .fill(0)
    .map((_, rowIndex) =>
      new Array(colCount)
        .fill(0)
        .map((_, colIndex) => `${rowIndex + 10}_${colIndex + 10}`)
    )
    .sort(() => Math.random() > 0.5)

const COL_COUNT = 20

export default {
  name: 'home',
  data() {
    return {
      bshd: null,
      bsx: null,
      bsy: null,
      beforePullDown: true,
      isPullingDown: false,
      isPullUpLoad: false,
      data: [],
      page: 0,
      fixedColCount: 2,
      cols: new Array(COL_COUNT).fill(0).map((_, index) => `Label${index}`)
    }
  },
  created() {
    this.getData(() => {
      this.$nextTick(this.initBS)
    })
  },
  methods: {
    initBS() {
      this.bshd = new BScroll('.bshd', {
        scrollX: true
      })
      const bsy = new BScroll('.bsy', {
        scrollY: true,
        pullUpLoad: true,
        pullDownRefresh: true,
        probeType: 3
      })
      bsy.on('pullingDown', this.onPullDown)
      bsy.on('pullingUp', this.onPullingUp)
      this.bsy = bsy
      const bsx = new BScroll('.bsx', {
        scrollX: true,
        probeType: 3
      })
      bsx.on('scroll', this.onBdXScroll)
      bsx.on('scrollEnd', this.onBdXScroll)
      this.bsx = bsx
    },
    onBdXScroll({ x }) {
      this.bshd.scrollTo(x, 0)
    },
    onPullingUp() {
      this.isPullUpLoad = true
      this.page++
      this.getData(() => {
        this.isPullUpLoad = false
        this.bsy.finishPullUp()
        this.bsy.refresh()
      })
    },
    onPullDown() {
      this.beforePullDown = false
      this.isPullingDown = true
      this.page = 0
      this.getData(() => {
        this.beforePullDown = true
        this.isPullingDown = false
        this.bsy.finishPullDown()
        this.bsy.refresh()
      })
    },
    getData(cb) {
      window.setTimeout(() => {
        const data = createData(
          parseInt(Math.random() * 40 + 30, 10),
          COL_COUNT
        )
        if (this.page === 0) {
          this.data = data
        } else {
          this.data = [...this.data, ...data]
        }
        cb && cb()
      }, parseInt(1 + Math.random() * 2, 10) * 1000)
    }
  },
  components: {
    PulldownLoader,
    PullupLoader
  }
}
</script>
