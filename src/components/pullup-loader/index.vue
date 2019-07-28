<template>
  <div class="pullup-wrapper txt-center txt-mute">
    <div v-if="!isPullUpLoad" class="before-trigger">
      <span class="pullup-txt">Pull up and load more</span>
    </div>
    <div v-else class="after-trigger">
      <span class="pullup-txt">Loading...</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'pullup-loader',
  props: ['isPullUpLoad']
}
</script>
