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
  font-size: 1.75vw;
  background: transparent;
  border: none;
  text-align: left;
  display: flex;
  align-items: stretch;
  justify-content: center;
  line-height: 1;
  overflow: hidden;
  padding: 0.5vw 0.75vw;

  .icon {
    width: 3.25vw;
    background-image: url('../assets/retry.svg');
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
