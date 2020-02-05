<template>
  <div class="prize-content">
    <template v-if="!showMore">
      <h1>
        <rainbow-text :text="$t('congrats')" />
      </h1>
      <h2>
        {{ $t('you-won') }}
        <span v-text="randomPrize.title"></span>
      </h2>
      <div class="icon"></div>
      <div>
        <button @click="onMoreClick">{{ $t('i-want-to-know-more') }}</button>
      </div>
    </template>
    <template v-else>
      <p class="more-text" v-text="randomPrize.description"></p>
    </template>
  </div>
</template>

<script>
import sample from 'lodash-es/sample.js';
import RainbowText from '@/components/RainbowText.vue';
import prizesJson from '@/assets/prizes.json';

export default {
  components: {
    RainbowText,
  },
  data() {
    return {
      showMore: false,
      randomPrize: {
        title: 'null',
        description: 'null',
      },
    };
  },
  mounted() {
    this.randomPrize = sample(prizesJson.prizes);
  },
  methods: {
    onMoreClick() {
      this.showMore = true;
    },
  },
};
</script>

<style lang="scss" scoped>
.prize-content {
  padding: 3em;
  text-align: center;
  user-select: none;
  display: flex;
  flex-direction: column;
  height: 100%;
  justify-content: center;
  position: relative;
  z-index: 100;
  color: #000;

  h1 {
    font-family: Montserrat, sans-serif;
    font-weight: 900;
    font-style: italic;
    font-size: 4em;
    letter-spacing: 0.1em;
    margin: 0.3em 0;

    @media all and (orientation: portrait) {
      margin-top: 1em;
    }
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
    margin: 3em 0;
  }

  button {
    background: transparent;
    border: 0.3em solid #fa71c6;
    padding: 0.75em 1.25em;
    font-family: Montserrat, sans-serif;
    font-style: italic;
    font-weight: 700;
    font-size: 1.25em;
    transition: border-color 0.15s ease, background-color 0.25s ease;

    @media all and (orientation: portrait) {
      font-size: 1.75em;
    }

    &:hover {
      transition: border-color 0.15s ease;
      border-color: #f8ed43;
    }

    @at-root {
      body.desaturated & {
        transition: border-color 2s linear, background-color 0.25s ease;
        border-color: #000;

        &:hover {
          background-color: #fff;
        }
      }
    }
  }

  .more-text {
    font-family: Montserrat, sans-serif;
    font-size: 1.75em;
    line-height: 1.4;
    overflow-y: auto;
    padding: 0 1.5em;
    margin: 0 -1.5em;

    @media all and (orientation: portrait) {
      font-size: 2.2em;
    }
  }
}
</style>
