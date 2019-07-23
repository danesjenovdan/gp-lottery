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
  font-size: 1.75em;
  background: #fff;
  border: none;
  text-align: left;
  display: flex;
  align-items: stretch;
  justify-content: center;
  line-height: 1;
  // overflow: hidden;
  padding: 0.75em 1em;
  position: relative;
  transition: all 0.15s ease;
  transform-style: preserve-3d;

  @media all and (orientation: portrait) {
    font-size: 3.2em;
    margin: 0 auto;
  }

  &::before,
  &::after {
    content: '';
    position: absolute;
    top: 0.42em;
    right: -0.85em;
    width: 0.85em;
    height: 100%;
    background: #fa71c6;
    transform: skewY(45deg) translateZ(-1px);
    transition: all 0.15s ease, background-color 2s linear;

    @at-root {
      body.desaturated & {
        background: #212c21;
      }
    }
  }

  &::after {
    height: 0.85em;
    width: 100%;
    bottom: -0.85em;
    left: 0.45em;
    top: auto;
    right: auto;
    transform: skewX(45deg) translateZ(-1px);
  }

  &:active {
    transform: translate(0.4em, 0.4em);

    &::before {
      top: 0.25em;
      right: -0.45em;
      width: 0.45em;
    }

    &::after {
      left: 0.25em;
      bottom: -0.45em;
      height: 0.45em;
    }
  }

  .icon {
    width: 3.25em;
    background-image: url('../assets/cta.svg');
    background-repeat: no-repeat;
    background-position: center;
    background-size: contain;
    margin-right: 0.75em;
  }

  .text {
    width: min-content;
  }
}
</style>
