<template>
  <div id="app">
    <transition name="slide-from-right" @after-enter="afterLogoEnter">
      <lottery-logo v-if="showLogo" :in-corner="showLogoInCorner" />
    </transition>
    <transition name="fade">
      <scratch-card v-if="showScratchCard" :page-width="pageWidth" />
    </transition>
    <!-- <transition name="fade">
      <call-to-action v-if="showCallToAction" :page-width="pageWidth" />
    </transition>-->
  </div>
</template>

<script>
import debounce from 'lodash-es/debounce.js';
import bus from '@/event-bus.js';
import resizeMixin from '@/mixins/resize.js';
import LotteryLogo from '@/components/LotteryLogo.vue';
import ScratchCard from '@/components/ScratchCard.vue';
// import CallToAction from '@/components/CallToAction.vue';

export default {
  name: 'app',
  components: {
    LotteryLogo,
    ScratchCard,
    // CallToAction,
  },
  mixins: [resizeMixin],
  data() {
    return {
      showLogo: false,
      showLogoInCorner: false,
      showScratchCard: false,
      showCallToAction: false,
      pageWidth: 0,
    };
  },
  mounted() {
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
      this.pageWidth = window.innerWidth;
    }, 150),
    // onShowCTA(data) {
    //   // this.showCallToAction = data;
    // },
    onDesaturate(data) {
      if (typeof window !== 'undefined' && document.body) {
        if (data) {
          document.body.classList.add('desaturated');
        } else {
          document.body.classList.remove('desaturated');
        }
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
  padding: 1em;
  height: 100%;
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
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
