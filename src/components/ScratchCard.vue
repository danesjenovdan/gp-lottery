<template>
  <div class="scratch-card-wrapper" :style="{ fontSize }">
    <div class="spacer-top"></div>
    <div class="scratch-card-container">
      <div class="scratch-card" ref="scratchCard">
        <scratch-card
          :key="renderCount"
          :cardWidth="cardWidth"
          :cardHeight="cardHeight"
          :finishPercent="finishPercent"
          imageUrl="img/scratch-cover.png"
          brushUrl="img/scratch-brush.png"
          @complete="onScratchComplete"
        >
          <prize-content />
        </scratch-card>
      </div>
    </div>
    <div class="spacer">
      <transition name="fade">
        <retry-button v-if="scratched" text="SCRATCH ANOTHER" @click.native="onRetryClick" />
      </transition>
    </div>
  </div>
</template>

<script>
import debounce from 'lodash-es/debounce.js';
import ScratchCard from '@/components/vue-scratchcard/ScratchCard.vue';
import RetryButton from '@/components/RetryButton.vue';
import PrizeContent from '@/components/PrizeContent.vue';
import resizeMixin from '@/mixins/resize.js';
import bus from '@/event-bus.js';

export default {
  components: {
    ScratchCard,
    RetryButton,
    PrizeContent,
  },
  mixins: [resizeMixin],
  props: {
    pageWidth: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      renderCount: 0,
      cardWidth: 0,
      cardHeight: 0,
      finishPercent: 60,
      scratched: false,
      showMore: false,
    };
  },
  methods: {
    onResize: debounce(function() {
      if (!this.scratched) {
        this.renderCount++;
      }
      this.cardWidth = this.$refs.scratchCard.clientWidth;
      this.cardHeight = this.$refs.scratchCard.clientHeight;
    }, 150),
    onScratchComplete() {
      bus.$emit('show-cta', true);
      bus.$emit('desaturate', true);
      this.scratched = true;
    },
    onRetryClick() {
      bus.$emit('show-cta', false);
      bus.$emit('desaturate', false);
      this.scratched = false;
      this.showMore = false;
      this.renderCount++;
    },
    onMoreClick() {
      this.showMore = true;
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
  width: 50%;
  margin: 0 5%;
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
    justify-content: flex-start;
    margin-top: 8em;
    position: relative;
    z-index: 2;
  }

  .scratch-card-container {
    display: flex;
    width: 100%;
    height: 50em;
    border: 1px solid #f8ed43;
    box-shadow: 0 0 3.5em rgba(86, 6, 76, 0.35);
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
    transition: border-color 2s linear;

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
      transition: opacity 2s linear;
    }

    @media all and (orientation: portrait) {
      height: 78em;
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
      margin: 2.2em;
      border: 0.52em solid #000;
      border-radius: 2.1em;
      background-color: #fff;
      overflow: hidden;

      @media all and (orientation: portrait) {
        margin: 2.6vh;
        border-width: 0.7vh;
        border-radius: 2.6vh;
      }

      .scratchcard {
        width: 100% !important;
        height: 100% !important;
      }
    }
  }

  .spacer {
    height: 5em;
  }
}
</style>
