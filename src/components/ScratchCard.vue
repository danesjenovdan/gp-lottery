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
        >
          <div class="scratch-content">
            <rainbow-text text="CONGRATS!" />
          </div>
        </scratch-card>
      </div>
    </div>
  </div>
</template>

<script>
import ScratchCard from 'vue-scratchcard/src/ScratchCard.vue';
import RainbowText from '@/components/RainbowText.vue';
import resizeMixin from '@/mixins/resize.js';

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
      finishPercent: 50,
    };
  },
  methods: {
    onResize() {
      this.renderCount++;
      this.cardWidth = this.$refs.scratchCard.clientWidth;
      this.cardHeight = this.$refs.scratchCard.clientHeight;
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
    background: linear-gradient(
      100deg,
      rgba(95, 166, 255, 1) 0%,
      rgba(106, 255, 192, 1) 20%,
      rgba(227, 255, 95, 1) 50%,
      rgba(255, 174, 95, 1) 80%,
      rgba(199, 0, 234, 1) 100%
    );

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
    }
  }
}
</style>
