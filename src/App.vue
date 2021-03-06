<template>
  <div id="app">
    <transition name="slide-from-right" @after-enter="afterLogoEnter">
      <lottery-logo v-if="showLogo" :in-corner="showLogoInCorner" />
    </transition>
    <div class="page-ratio">
      <transition name="fade">
        <scratch-card v-if="isScratch && showScratchCard" :page-width="pageWidth" />
        <spinner-wheel v-if="!isScratch && showScratchCard" :page-width="pageWidth" />
      </transition>
      <transition name="fade">
        <call-to-action v-if="showCallToAction" :page-width="pageWidth" />
      </transition>
    </div>
    <div class="video-container">
      <video src="video/12_Thick_Atmosphere_420_loop.mp4" autoplay muted loop />
    </div>
  </div>
</template>

<script>
import debounce from 'lodash-es/debounce.js';
import bus from '@/event-bus.js';
import resizeMixin from '@/mixins/resize.js';
import LotteryLogo from '@/components/LotteryLogo.vue';
import ScratchCard from '@/components/ScratchCard.vue';
import SpinnerWheel from '@/components/SpinnerWheel.vue';
import CallToAction from '@/components/CallToAction.vue';

export default {
  name: 'app',
  components: {
    LotteryLogo,
    ScratchCard,
    SpinnerWheel,
    CallToAction,
  },
  mixins: [resizeMixin],
  data() {
    return {
      isScratch: false,
      showLogo: false,
      showLogoInCorner: false,
      showScratchCard: false,
      showCallToAction: false,
      pageWidth: 0,
      desturateTimeout: null,
    };
  },
  mounted() {
    this.isScratch = window.location.search.indexOf('scratch') != -1;

    setTimeout(() => {
      this.showLogo = true;
    }, 200);

    bus.$on('show-cta', this.onShowCTA);
    bus.$on('desaturate', this.onDesaturate);
  },
  beforeDestroy() {
    bus.$off('show-cta', this.onShowCTA);
    bus.$off('desaturate', this.onDesaturate);
  },
  methods: {
    onResize: debounce(function() {
      if (window.innerHeight / window.innerWidth > 1.77) {
        this.pageWidth = window.innerWidth * 1.77;
      } else if (window.innerWidth / window.innerHeight > 1.77) {
        this.pageWidth = window.innerHeight * 1.77;
      } else {
        this.pageWidth =
          window.innerWidth < window.innerHeight ? window.innerHeight : window.innerWidth;
      }
    }, 150),
    onShowCTA(data) {
      this.showCallToAction = data;
    },
    onDesaturate(data) {
      if (data) {
        this.desturateTimeout = setTimeout(() => {
          document.body.classList.add('desaturated');
        }, 3000);
      } else {
        clearTimeout(this.desturateTimeout);
        document.body.classList.remove('desaturated');
      }
    },
    afterLogoEnter() {
      setTimeout(() => {
        this.showLogoInCorner = true;
      }, 1000);
      setTimeout(() => {
        this.showScratchCard = true;
      }, 2000);
    },
  },
};
</script>

<style lang="scss" scoped>
#app {
  position: relative;
  min-height: 100%;
  padding-top: 1px;
  overflow: hidden;
}

.page-ratio {
  width: 100vw;
  height: 56.25vw; /* 9/16 */
  max-height: 100vh;
  max-width: 177.78vh; /* 16/9 */
  margin: auto;
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  // z-index: 2;

  @media all and (orientation: portrait) {
    width: auto;
    max-width: initial;
    height: auto;
    max-height: initial;
    position: relative;
    // z-index: 2;
  }
}

.video-container {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 99;
  pointer-events: none;
  opacity: 0;
  // mix-blend-mode: multiply;
  transition: opacity 2s linear;

  video {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
  }

  @at-root {
    body.desaturated & {
      opacity: 0.6;
    }
  }
}

.slide-from-right-enter-active,
.slide-from-right-leave-active {
  transition: transform 1.5s ease;
}

.slide-from-right-enter,
.slide-from-right-leave-to {
  transform: translateX(5000px) translateY(2500px);
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
  transition-delay: 150ms;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
