<template>
  <div id="app">
    <transition name="slide-from-right" @after-enter="afterLogoEnter">
      <lottery-logo v-if="showLogo" :in-corner="showLogoInCorner" />
    </transition>
    <transition name="fade">
      <scratch-card v-if="showScratchCard" />
    </transition>
    <transition name="fade">
      <call-to-action v-if="showScratchCard" />
    </transition>
  </div>
</template>

<script>
import bus from '@/event-bus.js';
import LotteryLogo from '@/components/LotteryLogo.vue';
import ScratchCard from '@/components/ScratchCard.vue';
import CallToAction from '@/components/CallToAction.vue';

export default {
  name: 'app',
  components: {
    LotteryLogo,
    ScratchCard,
    CallToAction,
  },
  data() {
    return {
      showLogo: false,
      showLogoInCorner: false,
      showScratchCard: false,
    };
  },
  mounted() {
    setTimeout(() => {
      this.showLogo = true;
    }, 200);

    bus.$on('desaturate', this.onDesaturate);
  },
  beforeDestroy() {
    bus.$off('desaturate', this.onDesaturate);
  },
  methods: {
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

<style lang="scss">
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
  overscroll-behavior: none;
  overflow: hidden;
}

body {
  background-color: #f8ed43;
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

body.desaturated {
  background-color: #6a766a;
}

#app {
  overflow: hidden;
  position: fixed;
  top: 0;
  bottom: 1px;
  left: 0;
  right: 0;
}

button {
  cursor: pointer;
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.slide-from-right-enter-active,
.slide-from-right-leave-active {
  transition: transform 1.5s ease;
}

.slide-from-right-enter,
.slide-from-right-leave-to {
  transform: translateX(5000px) translateY(2500px);
}
</style>
