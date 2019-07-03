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
  position: relative;
  z-index: 0;
  font-size: 4rem;
  font-family: Montserrat, sans-serif;
  font-weight: 900;
  font-style: italic;
  letter-spacing: 6.3px;

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
    left: 4px;
    top: 4px;
    z-index: -1;
  }
}
</style>
