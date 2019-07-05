<template>
  <div class="scratch-card-wrapper">
    <div class="scratch-card-container">
      <div class="scratch-card" ref="scratchCard">
        <scratch-card
          :key="renderCount"
          :cardWidth="cardWidth"
          :cardHeight="cardHeight"
          :finishPercent="finishPercent"
          imageUrl="/img/scratch-cover.png"
          brushUrl="/img/scratch-brush.png"
          @complete="onScratchComplete"
        >
          <div class="scratch-content" :style="{ fontSize }">
            <h1>
              <rainbow-text text="CONGRATS!" />
            </h1>
            <h2>
              You got
              <span>PNEUMONIA</span>
            </h2>
            <div class="icon"></div>
            <div>
              <button>I want to know more</button>
            </div>
          </div>
        </scratch-card>
      </div>
    </div>
  </div>
</template>

<script>
import debounce from 'lodash-es/debounce.js';
import ScratchCard from 'vue-scratchcard/src/ScratchCard.vue';
import RainbowText from '@/components/RainbowText.vue';
import resizeMixin from '@/mixins/resize.js';
import bus from '@/event-bus.js';

export default {
  components: {
    ScratchCard,
    RainbowText,
  },
  mixins: [resizeMixin],
  data() {
    return {
      renderCount: 0,
      cardWidth: 0,
      cardHeight: 0,
      finishPercent: 60,
      pageWidth: 0,
    };
  },
  methods: {
    onResize: debounce(function() {
      this.renderCount++;
      this.cardWidth = this.$refs.scratchCard.clientWidth;
      this.cardHeight = this.$refs.scratchCard.clientHeight;
      this.pageWidth = window.innerWidth;
    }, 150),
    onScratchComplete() {
      bus.$emit('desaturate', true);
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
.scratch-card-wrapper {
  width: 55.6%;
  margin: 6.66%;
  display: flex;
  align-items: center;
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;

  .scratch-card-container {
    display: flex;
    width: 100%;
    height: 42.5vw;
    border: 1px solid #f8ed43;
    box-shadow: 0 0 51px rgba(86, 6, 76, 0.35);
    background-image: linear-gradient(
      100deg,
      rgba(95, 166, 255, 1) 0%,
      rgba(106, 255, 192, 1) 20%,
      rgba(227, 255, 95, 1) 50%,
      rgba(255, 174, 95, 1) 80%,
      rgba(199, 0, 234, 1) 100%
    );
    position: relative;
    z-index: 1;
    transition: border-color 0.5s cubic-bezier(1, 0.5, 0.8, 1);

    &::after {
      background-image: linear-gradient(100deg, #444a44 0%, #242f24 50%, #7f837f 100%);
      opacity: 0;
      position: absolute;
      content: '';
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      z-index: -1;
      transition: opacity 0.5s cubic-bezier(1, 0.5, 0.8, 1);
    }

    @at-root {
      body.desaturated & {
        border-color: #6a766a;

        &::after {
          opacity: 1;
        }
      }
    }

    .scratch-card {
      flex: 1;
      margin: 2.2vw;
      border: 0.52vw solid #000;
      border-radius: 2.1vw;
      background-color: #fff;
      overflow: hidden;

      .scratchcard {
        width: 100% !important;
        height: 100% !important;
      }
    }

    .scratch-content {
      padding: 2vw;
      text-align: center;
      user-select: none;

      h1 {
        font-family: Montserrat, sans-serif;
        font-weight: 900;
        font-style: italic;
        font-size: 4em;
        letter-spacing: 0.1em;
        margin: 0.3em 0;
      }

      h2 {
        font-family: Montserrat, sans-serif;
        font-weight: 400;
        font-style: italic;
        font-size: 2.5em;
        letter-spacing: 0.1em;
        margin: 0.5em 0;

        span {
          font-size: 1.2em;
          font-weight: 900;
        }
      }

      .icon {
        background-image: url('../assets/lungs.svg');
        background-repeat: no-repeat;
        background-position: center;
        background-size: contain;
        width: 100%;
        height: 17em;
        margin: 2em 0 3em 0;
      }

      button {
        background: transparent;
        border: 0.3em solid #fa71c6;
        padding: 0.75em 1.25em;
        font-family: Montserrat, sans-serif;
        font-style: italic;
        font-weight: 700;
        font-size: 1.25em;
        transition: border-color 0.5s cubic-bezier(1, 0.5, 0.8, 1);

        @at-root {
          body.desaturated & {
            border-color: #000;
          }
        }
      }
    }
  }
}
</style>
