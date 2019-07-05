<template>
  <div class="small-logo"></div>
</template>

<style lang="scss" scoped>
.small-logo {
  position: absolute;
  top: 2.5rem;
  right: 0;
  width: 22vw;
  height: 360px;
  background-image: url('../assets/logo-small.png');
  background-repeat: no-repeat;
  background-position: right top;
  background-size: 100%;
  transition: filter 0.5s cubic-bezier(1, 0.5, 0.8, 1);

  @at-root {
    body.desaturated & {
      filter: contrast(0.5) grayscale(0.75) brightness(0.75);
    }
  }
}
</style>
