<template>
  <div class="position-relative">
    <div id="overlay"></div>
    <div :style="{ backgroundImage: `url(${bgImg})` }"></div>
  </div>
</template>

<script>
export default {
  name: 'TopPhoto',
  props: {
    bgImg: {
      type: undefined,
      required: true,
    },
  },
}
</script>
<style lang="scss" scoped>
div {
  height: 220px;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center center;
}

#overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.4);
  z-index: 2;
}
</style>
