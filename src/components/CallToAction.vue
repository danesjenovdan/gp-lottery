<template>
  <div class="call-to-action-wrapper">
    <div class="spacer-top"></div>
    <div class="call-to-action-container">
      <div class="action-content" :style="{ fontSize }">
        <p>
          Harmful effects of air pollution occur even at levels below air quality standards
          considered to be safe by official institutions. Most air pollution comes from burning
          fossil fuels such as coal to generate electricity and heat our homes.
          <em>The best way to reduce human exposure to air pollution is to</em>
          <em>control it at its source.</em>
        </p>
        <div class="action-button">
          <cta-button text="TAKE ACTION" />
        </div>
      </div>
      <div class="action-socials" :style="{ fontSize }">
        <hr />
        <div class="action-socials-content">
          <div class="buttons">
            <div class="warn-text">
              WARN YOUR
              <br />FRIENDS!
            </div>
            <button class="fb">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="7 4 9 16" fill="currentColor">
                <path
                  d="M13 10h3v3h-3v7h-3v-7H7v-3h3V8.745c0-1.189.374-2.691 1.118-3.512C11.862 4.41 12.791 4 13.904 4H16v3h-2.1c-.498 0-.9.402-.9.899V10z"
                />
              </svg>
            </button>
            <button class="tw">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="2.999 4.699 18.001 14.601"
                fill="currentColor"
              >
                <path
                  d="M18.89 7.012c.808-.496 1.343-1.173 1.605-2.034a8.68 8.68 0 0 1-2.351.861c-.703-.756-1.593-1.14-2.66-1.14-1.043 0-1.924.366-2.643 1.078a3.56 3.56 0 0 0-1.076 2.605c0 .309.039.585.117.819C8.806 9.096 6.26 7.82 4.254 5.364c-.34.601-.51 1.213-.51 1.846 0 1.301.549 2.332 1.645 3.089-.625-.053-1.176-.211-1.645-.47 0 .929.273 1.705.82 2.388a3.623 3.623 0 0 0 2.115 1.291c-.312.08-.641.118-.979.118-.312 0-.533-.026-.664-.083.23.757.664 1.371 1.291 1.841a3.652 3.652 0 0 0 2.152.743c-1.332 1.045-2.855 1.562-4.578 1.562-.422 0-.721-.006-.902-.038C4.696 18.753 6.585 19.3 8.675 19.3c2.139 0 4.029-.542 5.674-1.626 1.645-1.078 2.859-2.408 3.639-3.974a10.77 10.77 0 0 0 1.172-4.892V8.34A7.788 7.788 0 0 0 21 6.419a8.142 8.142 0 0 1-2.11.593z"
                />
              </svg>
            </button>
            <button class="email">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="56 114.965 400.03 282.165"
                fill="currentColor"
              >
                <path
                  d="M255.78 397.11H90.52c-17.18 0-30.21-9.84-33.77-25.41a37 37 0 0 1-.71-8.17Q56 256.24 56 149c0-20.57 13.78-34 34.93-34q165.26-.07 330.53 0C442 114.92 456 128.53 456 148.5q.06 107.49 0 215c0 20.13-14 33.6-34.92 33.61q-82.66.04-165.3 0zm56.15-118.62a14.12 14.12 0 0 0 2.16 3.48q28.76 28 57.65 55.8c5.07 4.9 10.05 9.9 15.25 14.67a15.65 15.65 0 0 0 16.57 3.46 15.16 15.16 0 0 0 10.58-12.38c1-5.95-1.51-10.59-5.79-14.72q-35.72-34.41-71.35-68.93c-1.21-1.17-2.36-2.4-4.09-4.17a52.77 52.77 0 0 0 4.28-3.33q35.1-34.09 70.14-68.23c.95-.92 1.93-1.81 2.81-2.79 8.34-9.26 3.67-23.13-8.7-25.81-6.49-1.41-11.55 1.07-16.07 5.45q-59 57.22-118.13 114.36c-8.14 7.88-14.51 7.91-22.59.08l-116.8-113.17a49.31 49.31 0 0 0-3.88-3.59c-9.33-7.27-23-2.7-25.85 8.58-1.61 6.45.9 11.49 5.53 16q35.73 34.7 71.36 69.5c1.12 1.1 2.19 2.25 3.43 3.53-1.34 1.36-2.43 2.52-3.58 3.63q-35.63 34.49-71.3 68.95c-4.17 4-6.65 8.57-5.8 14.32a15.11 15.11 0 0 0 10.72 12.75c6.74 2.27 12.58.42 17.56-4.41Q162 316.69 198 281.92c1.13-1.09 2.31-2.13 3.21-3 9.07 8.6 17.87 16.85 26.55 25.19 17.28 16.6 39.48 16.55 56.68 0 8.62-8.33 17.56-16.4 27.49-25.62z"
                />
              </svg>
            </button>
            <div class="input">
              <input type="text" value="http://gps.si/wat" onfocus="this.select()" />
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

  @media all and (orientation: portrait) {
    width: auto;
    right: 0;
    margin: 0 5%;
    position: relative;
    // z-index: 1;
    margin-bottom: 4em;
  }

  .call-to-action-container {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 42.5vw;
    position: relative;
    // z-index: 1;

    @media all and (orientation: portrait) {
      height: auto;
    }

    .action-content {
      display: flex;
      flex-direction: column;
      height: 100%;
      margin-top: 9em;
      transition: color 2s linear;
      z-index: 100;

      @at-root {
        body.desaturated & {
          color: #fff;
        }
      }

      p {
        font-family: Montserrat, sans-serif;
        font-size: 1.5em;
        line-height: 1.5;

        @media all and (orientation: portrait) {
          font-size: 2.2em;
          text-align: center;
        }

        em {
          font-weight: 700;
          font-style: italic;
        }
      }

      .action-button {
        margin: 2em 0;

        @media all and (orientation: portrait) {
          margin-bottom: 4em;
        }

        button {
          transition: box-shadow 0.25s ease;

          &:focus {
            outline: none;
          }

          &:hover {
            box-shadow: 0 0 16px 0px rgba(255, 255, 255, 0.5);
          }
        }
      }
    }

    .action-socials {
      display: flex;
      flex-direction: column;

      hr {
        border: 0;
        border-top: 2px solid #fa71c6;
        height: 0;
        margin: 1.5em 0;
        transition: border-top-color 2s linear;

        &:last-child {
          margin-bottom: 0;
        }

        @at-root {
          body.desaturated & {
            border-top-color: #212c21;
          }
        }
      }

      .action-socials-content {
        display: flex;

        .buttons {
          display: flex;
          font-family: Montserrat, sans-serif;
          flex: 1;
          justify-content: center;
          align-items: center;
          flex-wrap: wrap;

          .warn-text {
            font-family: Montserrat, sans-serif;
            font-weight: 900;
            font-size: 1.5em;
            font-style: italic;
            line-height: 1;
            flex-shrink: 0;
            margin-right: 0.5em;
            transition: color 2s linear;

            @media all and (orientation: portrait) {
              font-size: 2.5em;
            }

            @at-root {
              body.desaturated & {
                color: #fff;
              }
            }
          }

          button {
            background: #fff;
            border: 0;
            color: #fa71c6;
            flex-basis: 3em;
            flex-shrink: 0;
            flex-grow: 0;
            font-family: inherit;
            font-size: inherit;
            margin: 0 0.5em 0 0;
            padding: 0.5em;
            width: 3em;
            height: 3em;
            transition: color 0.15s ease;
            z-index: 100;

            @media all and (orientation: portrait) {
              flex-basis: 6em;
              width: 6em;
              height: 6em;
              margin-right: 1.5em;
              padding: 1em;
            }

            &:hover {
              color: #f8ed43;
            }

            svg {
              width: 100%;
              height: 100%;
            }

            @at-root {
              body.desaturated & {
                transition: color 2s linear, background-color 0.15s ease;
                color: #212c21;
                background-color: rgba(255, 255, 255, 0.5);

                &:hover {
                  background-color: #fff;
                }
              }
            }
          }

          .input {
            flex-basis: 3em;
            flex-shrink: 0;
            flex-grow: 1;

            @media all and (orientation: portrait) {
              flex-basis: 100%;
              margin-top: 1em;
              padding: 0 8em;
            }

            input {
              border: 0.3em solid #fa71c6;
              background: transparent;
              font-family: inherit;
              font-size: inherit;
              font-weight: 500;
              font-style: italic;
              padding: 0.32em 1em;
              width: 100%;
              height: 100%;
              text-align: center;
              font-size: 1.15em;
              transition: border-color 2s linear, color 2s linear;

              @media all and (orientation: portrait) {
                font-size: 2.5em;
                padding: 0.5em 1em;
              }

              @at-root {
                body.desaturated & {
                  border-color: #fff;
                  color: #fff;
                }
              }
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
