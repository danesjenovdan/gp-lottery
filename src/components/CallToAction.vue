<template>
  <div class="call-to-action-wrapper">
    <div class="spacer-top"></div>
    <div class="call-to-action-container">
      <div class="action-content" :style="{ fontSize }">
        <p>
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Tempore, minima architecto? Saepe
          eaque eligendi ipsa tempora quo.Libero, assumenda? Enim delectus cupiditate cum
          consectetur dolor praesentium ea ad, quae dolorem error assumenda quibusdam,
          <em>
            at culpa esse incidunt ipsum nulla vero sapiente deserunt possimus? Autem laboriosam
            doloremque alias.
          </em>
        </p>
        <div>
          <cta-button text="TAKE ACTION" />
        </div>
      </div>
      <div class="action-socials" :style="{ fontSize }">
        <hr />
        <div>yxo</div>
        <hr />
      </div>
    </div>
    <div class="spacer"></div>
  </div>
</template>

<script>
import CtaButton from '@/components/CtaButton.vue';

export default {
  components: {
    CtaButton,
  },
  computed: {
    fontSize() {
      const size = (this.pageWidth / 1920) * 16;
      return `${size}px`;
    },
  },
};
</script>

<style lang="scss" scoped>
.call-to-action-wrapper {
  width: 30%;
  margin: 0 58% 0;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;

  .call-to-action-container {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 42.5vw;
    // box-shadow: 0 0 51px rgba(86, 6, 76, 0.35);
    position: relative;
    z-index: 1;
    // transition: border-color 0.5s cubic-bezier(1, 0.5, 0.8, 1);

    .action-content {
      display: flex;
      flex-direction: column;
      height: 100%;
      margin-top: 9em;

      p {
        font-family: Montserrat, sans-serif;
        font-size: 1.5em;
        line-height: 1.5;

        em {
          font-weight: 700;
          font-style: italic;
        }
      }
    }

    .action-socials {
      display: flex;
      flex-direction: column;

      hr {
        border-top: 1px solid #fa71c6;
        height: 0;
        margin: 1.5em 0;

        &:last-child {
          margin-bottom: 0;
        }
      }
    }
  }

  .spacer {
    height: 5vw;
  }
}
</style>
