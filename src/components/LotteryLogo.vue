<template>
  <div :class="['lottery-logo', { 'lottery-logo--in-corner': inCorner }]"></div>
</template>

<script>
export default {
  props: {
    inCorner: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="scss" scoped>
.lottery-logo {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-image: url('../assets/lottery-logo.png');
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center center;
  transform-origin: center;
  transform: scale(1) translateY(15vw);
  transition: all 1.5s;
  transition-timing-function: ease;

  @media all and (orientation: portrait) {
    transform: scale(1.75) translateY(15vw);
  }

  &.lottery-logo--in-corner {
    background-position: top 4vw center;
    transform-origin: top;
    transform: scale(0.4) translateX(96%);

    @media all and (orientation: portrait) {
      background-position: bottom center;
      transform-origin: bottom;
      transform: scale(1) translateY(35vw);
    }
  }
}
</style>
