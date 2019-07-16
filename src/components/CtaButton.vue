<template>
  <button class="cta-button">
    <div class="icon"></div>
    <div class="text" v-text="text"></div>
  </button>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.cta-button {
  font-family: Montserrat, sans-serif;
  font-style: italic;
  font-weight: 900;
  font-size: 1.75vw;
  background: #fff;
  border: none;
  text-align: left;
  display: flex;
  align-items: stretch;
  justify-content: center;
  line-height: 1;
  // overflow: hidden;
  padding: 0.75vw 1vw;
  position: relative;
  transition: all 0.15s ease;
  transform-style: preserve-3d;

  &::before,
  &::after {
    content: '';
    position: absolute;
    top: 9px;
    right: -19px;
    width: 19px;
    height: 100%;
    background: #fa71c6;
    transform: skewY(45deg) translateZ(-1px);
    transition: all 0.15s ease;
  }

  &::after {
    height: 19px;
    width: 100%;
    bottom: -19px;
    left: 10px;
    top: auto;
    right: auto;
    transform: skewX(45deg) translateZ(-1px);
  }

  &:active {
    transform: translate(10px, 10px);

    &::before {
      top: 4px;
      right: -9px;
      width: 9px;
    }

    &::after {
      bottom: -9px;
      left: 5px;
      height: 9px;
    }
  }

  .icon {
    width: 3.25vw;
    background-image: url('../assets/cta.svg');
    background-repeat: no-repeat;
    background-position: center;
    background-size: contain;
    margin-right: 0.75vw;
  }

  .text {
    width: min-content;
  }
}
</style>
