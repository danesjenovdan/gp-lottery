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
        <div class="action-socials-content">
          <div class="warn-text">
            WARN YOUR
            <br />FRIENDS!
          </div>
          <div class="buttons">
            <button class="fb"></button>
            <button class="tw"></button>
            <button class="email"></button>
            <div class="input">
              <input type="text" value="http://gps.si/wat" />
            </div>
          </div>
        </div>
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
  props: {
    pageWidth: {
      type: Number,
      required: true,
    },
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
        border-top: 2px solid #fa71c6;
        height: 0;
        margin: 1.5em 0;

        &:last-child {
          margin-bottom: 0;
        }
      }

      .action-socials-content {
        display: flex;

        .warn-text {
          font-family: Montserrat, sans-serif;
          font-weight: 900;
          font-size: 1.5em;
          font-style: italic;
          line-height: 1;
          flex-shrink: 0;
          margin-right: 0.5em;
        }

        .buttons {
          height: 3em;
          display: flex;
          font-family: Montserrat, sans-serif;
          flex: 1;

          button {
            background: #fff;
            border: 0;
            color: #fa71c6;
            flex-basis: 3em;
            flex-shrink: 0;
            flex-grow: 0;
            font-family: inherit;
            font-size: inherit;
            margin-right: 0.5em;
            background-repeat: no-repeat;
            background-position: center;
            background-size: 50% 50%;

            &.email {
              background-image: url('../assets/mail.svg');
            }

            &.fb {
              background-image: url('../assets/fb.svg');
            }

            &.tw {
              background-image: url('../assets/tw.svg');
            }
          }

          .input {
            flex-basis: 3em;
            flex-shrink: 0;
            flex-grow: 1;

            input {
              border: 3px solid #fa71c6;
              background: transparent;
              font-family: inherit;
              font-size: inherit;
              font-weight: 700;
              font-style: italic;
              padding: 0 1em;
              width: 100%;
              height: 100%;
              text-align: center;
              font-size: 1.15em;
            }
          }
        }
      }
    }
  }

  .spacer {
    height: 5vw;
  }
}
</style>
