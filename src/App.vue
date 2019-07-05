<template>
  <div id="app">
    <transition name="fade" @after-leave="afterSplashLeave">
      <splash-logo v-if="showSplash" />
    </transition>
    <transition name="fade">
      <small-logo v-if="!showSplash" />
    </transition>
    <transition name="fade">
      <scratch-card v-if="showContent" />
    </transition>
  </div>
</template>

<script>
import SplashLogo from '@/components/SplashLogo.vue';
import SmallLogo from '@/components/SmallLogo.vue';
import ScratchCard from '@/components/ScratchCard.vue';
import bus from '@/event-bus.js';

export default {
  name: 'app',
  components: {
    SplashLogo,
    SmallLogo,
    ScratchCard,
  },
  data() {
    return {
      showSplash: true,
      showContent: false,
      desaturated: false,
    };
  },
  mounted() {
    setTimeout(() => {
      this.showSplash = false;
    }, 2000);

    bus.$on('desaturate', this.onDesaturate);
  },
  beforeDestroy() {
    bus.$off('desaturate', this.onDesaturate);
  },
  methods: {
    afterSplashLeave() {
      this.showContent = true;
    },
    onDesaturate(data) {
      if (typeof window !== 'undefined' && document.body) {
        if (data) {
          document.body.classList.add('desaturated');
        } else {
          document.body.classList.remove('desaturated');
        }
      }
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
}

body {
  background-color: #f8ed43;
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

body.desaturated {
  background-color: #6a766a;
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
