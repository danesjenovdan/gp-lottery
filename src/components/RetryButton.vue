<template>
  <button class="retry-button">
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
.retry-button {
  font-family: Montserrat, sans-serif;
  font-style: italic;
  font-weight: 900;
  font-size: 1.75em;
  background: transparent;
  border: none;
  text-align: left;
  display: flex;
  align-items: stretch;
  justify-content: center;
  line-height: 1;
  overflow: hidden;
  padding: 0.5em 0.75em;
  transition: filter 2s linear;

  @media all and (orientation: portrait) {
    font-size: 2.2em;
    margin-top: 2em;
  }

  .icon {
    width: 3.25em;
    background-image: url('../assets/retry.svg');
    background-repeat: no-repeat;
    background-position: center;
    background-size: contain;
    margin-right: 0.75em;

    @media all and (orientation: portrait) {
      width: 4em;
    }
  }

  .text {
    width: min-content;
  }

  @at-root {
    body.desaturated & {
      filter: invert(1);
    }
  }
}
</style>
