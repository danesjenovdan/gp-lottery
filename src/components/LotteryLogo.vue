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
  position: absolute;
  top: 0;
  right: 0;
  bottom: -10vw;
  width: 100%;
  background-image: url('../assets/lottery-logo.png');
  background-repeat: no-repeat;
  background-position: left 75% bottom;
  background-size: 69vw;
  transition: all 1.5s ease;
  transform-origin: top right;

  @media all and (orientation: portrait) {
    background-position: left -30% bottom 30vw;
    background-size: 130vw;
  }

  &.lottery-logo--in-corner {
    background-position: top right;
    transform: scale(0.4) translateY(3vw) translateX(21%);
    bottom: -100vh;

    @media all and (orientation: portrait) {
      background-position: left 65% bottom -12vw;
      bottom: -10vw;
      transform: none;
      background-size: 50vw;
    }
  }
}
</style>
