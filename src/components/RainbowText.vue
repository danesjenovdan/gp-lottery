<template>
  <span class="rainbow-text-shadow" v-text="text" :data-text="text" />
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
.rainbow-text-shadow {
  display: inline-block;
  position: relative;
  z-index: 0;
  padding-right: 0.06em;
  color: #000;

  &::after {
    content: attr(data-text);
    background: linear-gradient(
      90deg,
      rgba(95, 166, 255, 1) 0%,
      rgba(106, 255, 192, 1) 20%,
      rgba(227, 255, 95, 1) 50%,
      rgba(255, 174, 95, 1) 80%,
      rgba(199, 0, 234, 1) 100%
    );
    background-clip: text;
    color: transparent;
    display: inline-block;
    position: absolute;
    left: 0.06em;
    top: 0.04em;
    z-index: -1;
    transition: opacity 2s linear;

    @at-root {
      body.desaturated & {
        opacity: 0;
      }
    }
  }
}
</style>
