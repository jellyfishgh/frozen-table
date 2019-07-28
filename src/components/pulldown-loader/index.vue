<template >
  <div class="pulldown-loader txt-center txt-mute absolute">
    <div v-show="beforePullDown">
      <span>Pull Down and refresh</span>
    </div>
    <div v-show="!beforePullDown">
      <div v-show="isPullingDown">
        <span>Loading...</span>
      </div>
      <div v-show="!isPullingDown">
        <span>Refresh success</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'pulldown-loader',
  props: ['beforePullDown', 'isPullingDown']
}
</script>

<style lang="less">
.pulldown-loader {
  left: 0;
  right: 0;
  top: 0;
  transform: translateY(-100%) translateZ(0);
}
</style>
