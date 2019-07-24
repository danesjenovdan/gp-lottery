<template>
  <div class="spinner-wheel-wrapper">
    <div :style="{ fontSize }" :class="['spinner-wheel', { 'spinner-wheel--spun': spun }]">
      <div class="spinner" @click="startSpin">
        <div class="spinner-list" :style="{ transform: `rotate(${this.startingDeg}deg)` }">
          <li v-for="i in 12" :key="`spinner-slice-${i}`" class="spinner-slice"></li>
        </div>
      </div>
      <div class="marker" @click="startSpin">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="26.667 5.03 89.646 50.446" fill="#2145d0">
          <path
            d="M116.313 32.012c0-12.938-10.526-23.463-23.464-23.463-12.263 0-64.125 19.047-66.182 19.944v3.52l3.387 1.477c2.057.897 50.532 21.986 62.795 21.986 12.938 0 23.464-10.526 23.464-23.464zM92.849 37.14a5.133 5.133 0 0 1-5.127-5.128c0-2.828 2.3-5.128 5.127-5.128s5.128 2.3 5.128 5.128c0 2.828-2.3 5.128-5.128 5.128z"
            fill="#0d2892"
          />
          <path
            d="M116.313 28.493c0-12.938-10.526-23.463-23.464-23.463-12.263 0-60.738 21.088-62.795 21.985l-3.387 1.478 3.387 1.478c2.057.897 50.532 21.986 62.795 21.986 12.938 0 23.464-10.526 23.464-23.464zm-23.464 5.128a5.133 5.133 0 0 1-5.127-5.128c0-2.828 2.3-5.128 5.127-5.128s5.128 2.3 5.128 5.128c0 2.828-2.3 5.128-5.128 5.128z"
          />
        </svg>
      </div>
      <transition name="fade">
        <retry-button v-if="spun" text="SPIN AGAIN" @click.native="onRetryClick" />
      </transition>
    </div>
    <transition name="fade">
      <div class="card-content" v-if="spun" :style="{ fontSize }">
        <prize-content />
      </div>
    </transition>
  </div>
</template>

<script>
import PrizeContent from '@/components/PrizeContent.vue';
import RetryButton from '@/components/RetryButton.vue';
import bus from '@/event-bus.js';

export default {
  components: {
    PrizeContent,
    RetryButton,
  },
  props: {
    pageWidth: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      startingDeg: 500,
      isSpinning: false,
      spun: false,
    };
  },
  methods: {
    onSpinComplete() {
      bus.$emit('show-cta', true);
      bus.$emit('desaturate', true);
      this.spun = true;
    },
    onRetryClick() {
      bus.$emit('show-cta', false);
      bus.$emit('desaturate', false);
      this.spun = false;
    },
    startSpin() {
      if (this.isSpinning || this.spun) {
        return;
      }

      this.isSpinning = true;
      this.startingDeg = this.startingDeg + Math.round(Math.random() * (3000 - 360) + 360);

      setTimeout(() => {
        this.isSpinning = false;
        this.onSpinComplete();
      }, 3000);
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
$pi: 3.14159265358979;

$spinner-size: 90vh;
$spinner-radius: $spinner-size / 2px;
$spinner-circumference: $pi * $spinner-size;
$spinner-slices: 12;
$slice-size: $spinner-circumference / $spinner-slices;

$slice-colors: (
  #a94fca,
  #ee4266,
  #ffd23f,
  #3bceac,
  #2765d4,
  #ff715b,
  #a94fca,
  #ee4266,
  #ffd23f,
  #3bceac,
  #2765d4,
  #ff715b
);

.spinner-wheel-wrapper {
  width: 50%;
  margin: 0 5%;
  display: flex;
  // flex-direction: column;
  justify-content: center;
  align-items: center;
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;

  @media all and (orientation: portrait) {
    //
  }

  .spinner-wheel {
    width: $spinner-size;
    transform: translateX(-60%);
    transition: transform 0.5s ease;

    &.spinner-wheel--spun {
      transform: translateX(-85%);

      .marker {
        cursor: initial;
      }
    }

    .spinner {
      position: relative;
      width: 100%;
      padding-bottom: 100%;

      // center dot
      &:after {
        content: '';
        display: block;
        width: 27vh;
        height: 27vh;
        background-color: #e5e5e5;
        position: absolute;
        border-radius: 50%;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        margin: auto;
        box-shadow: 0 0.6em 0 0 #b7b7b7;
      }

      .spinner-list {
        transition: transform 3000ms ease-out;
        position: absolute;
        width: 100%;
        height: 100%;
        overflow: hidden;
        border-radius: 50%;
        box-shadow: 0 0 3.5em rgba(86, 6, 76, 0.35);
      }

      .spinner-slice {
        position: absolute;
        display: flex;
        top: 50%;
        transform-origin: right;
        list-style: none;
        height: $slice-size;
        width: $spinner-size/2;
        justify-content: flex-start;
        align-items: center;
        z-index: -1;

        &:after {
          content: '';
          position: absolute;
          top: 0;
          width: 0;
          height: 0;
          border-left: $spinner-size/2 solid;
          border-bottom: $slice-size/1.95 solid transparent;
          border-top: $slice-size/1.95 solid transparent;
        }

        @for $i from 1 through $spinner-slices {
          &:nth-of-type(#{$i}) {
            transform: translateY(-50%) rotate(30deg * $i);
            &:after {
              border-left-color: nth($slice-colors, $i);
            }
          }
        }
      }
    }

    .marker {
      position: absolute;
      top: 50%;
      right: -12vh;
      width: 20vh;
      height: 20vh;
      transform: translateY(-50%);
      cursor: pointer;

      svg {
        width: 100%;
        height: 100%;
      }
    }

    .retry-button {
      position: absolute;
      top: 66%;
      right: -5em;
      transform: translateY(-50%);
      text-align: center;
      flex-direction: column;

      &::v-deep .icon {
        display: block;
        height: 2em;
        margin: 0 auto 0.5em;
      }
    }
  }

  .card-content {
    width: 66%;
    height: 70%;
    background: #fff;
    position: absolute;
    right: 0;
  }
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
