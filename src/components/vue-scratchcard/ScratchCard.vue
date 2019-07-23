<template>
  <div class="scratchcard" :style="`width:${cardWidth}px; height:${cardHeight}px`">
    <canvas
      @mousedown="handleMouseDown"
      @mousemove="handleMouseMove"
      @mouseup="handleMouseUp"
      @touchstart="handleMouseDown"
      @touchmove="handleMouseMove"
      @touchend="handleMouseUp"
      ref="canvas"
      class="scratchcard-overlay"
    ></canvas>
    <div v-if="overlayLoaded" class="scratchcard-content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
/**
 * By Ken Fyrstenberg Nilsen https://stackoverflow.com/a/21961894
 */
function drawImageCover(ctx, img, x, y, w, h) {
  var iw = img.width,
    ih = img.height,
    r = Math.min(w / iw, h / ih),
    nw = iw * r, // new prop. width
    nh = ih * r, // new prop. height
    cx,
    cy,
    cw,
    ch,
    ar = 1;

  // decide which gap to fill
  if (nw < w) ar = w / nw;
  if (Math.abs(ar - 1) < 1e-14 && nh < h) ar = h / nh; // updated
  nw *= ar;
  nh *= ar;

  // calc source rectangle
  cw = iw / (nw / w);
  ch = ih / (nh / h);

  cx = (iw - cw) * 0.5;
  cy = (ih - ch) * 0.5;

  // make sure source rectangle is valid
  if (cx < 0) cx = 0;
  if (cy < 0) cy = 0;
  if (cw > iw) cw = iw;
  if (ch > ih) ch = ih;

  // fill image in dest. rectangle
  ctx.drawImage(img, cx, cy, cw, ch, x, y, w, h);
}

function getFilledPercent(ctx, width, height, stride) {
  if (!stride || stride < 1) stride = 1;
  const pixels = ctx.getImageData(0, 0, width, height);
  const total = pixels.data.length / stride;

  let count = 0;
  for (let i = 0; i < pixels.data.length; i += stride) {
    if (parseInt(pixels.data[i], 10) === 0) count++;
  }
  return Math.round((count / total) * 100);
}

function getMouse(e, canvas) {
  const { left, top } = canvas.getBoundingClientRect();
  const touch = e.touches && e.touches[0];
  if (touch) {
    return { x: touch.clientX - left, y: touch.clientY - top };
  } else {
    const scrollTop = window.pageYOffset || document.documentElement.scrollTop;
    const scrollLeft = window.pageXOffset || document.documentElement.scrollLeft;
    return { x: e.pageX - left - scrollLeft, y: e.pageY - top - scrollTop };
  }
}

function distanceBetween(point1, point2) {
  return Math.sqrt(Math.pow(point2.x - point1.x, 2) + Math.pow(point2.y - point1.y, 2));
}

function angleBetween(point1, point2) {
  return Math.atan2(point2.x - point1.x, point2.y - point1.y);
}

export default {
  name: 'ScratchCard',

  props: {
    imageUrl: String,
    brushUrl: String,
    cardWidth: Number,
    cardHeight: Number,
    finishPercent: Number,
    forceReveal: Boolean,
    onComplete: Function,
  },

  data() {
    return {
      overlayLoaded: false,
      isDrawing: false,
      isFinished: false,
      canvas: undefined,
      ctx: undefined,
      lastPoint: undefined,
      brush: new Image(),
    };
  },

  methods: {
    initCanvas() {
      this.canvas = this.$refs.canvas;
      this.canvas.width = this.cardWidth;
      this.canvas.height = this.cardHeight;
      this.ctx = this.canvas.getContext('2d');
    },

    drawImage() {
      const image = new Image();
      image.crossOrigin = 'Anonymous';
      image.src = this.imageUrl;
      image.onload = () => {
        drawImageCover(this.ctx, image, 0, 0, this.cardWidth, this.cardHeight);
        this.overlayLoaded = true;
      };
    },

    prepBrush() {
      if (this.brushUrl) {
        this.brush.crossOrigin = 'Anonymous';
        this.brush.src = this.brushUrl;
      }
    },

    scratchAt(x, y) {
      if (this.brushUrl) {
        const brushSizeMultiplier = Math.min(this.cardWidth, this.cardHeight) / 500;
        this.ctx.drawImage(
          this.brush,
          x - (this.brush.width * brushSizeMultiplier) / 2,
          y - (this.brush.height * brushSizeMultiplier) / 2,
          this.brush.width * brushSizeMultiplier,
          this.brush.height * brushSizeMultiplier,
        );
      } else {
        this.ctx.beginPath();
        this.ctx.arc(x, y, 25, 0, 2 * Math.PI, false);
        this.ctx.fill();
      }
    },

    handleMouseDown(e) {
      this.isDrawing = true;
      this.lastPoint = getMouse(e, this.canvas);
    },

    handleMouseUp() {
      this.isDrawing = false;
    },

    handleMouseMove(e) {
      if (!this.isDrawing) return;

      e.preventDefault();
      const currentPoint = getMouse(e, this.canvas);
      const distance = distanceBetween(this.lastPoint, currentPoint);
      const angle = angleBetween(this.lastPoint, currentPoint);

      let x, y;
      for (let i = 0; i < distance; i++) {
        x = this.lastPoint.x + Math.sin(angle) * i;
        y = this.lastPoint.y + Math.cos(angle) * i;
        this.ctx.globalCompositeOperation = 'destination-out';
        this.scratchAt(x, y);
      }
      this.lastPoint = currentPoint;
      this.handlePercentage(getFilledPercent(this.ctx, this.cardWidth, this.cardHeight, 32));
    },

    handlePercentage(filledInPixels = 0) {
      if (filledInPixels > this.finishPercent) this.reveal();
    },

    reveal() {
      if (!this.isFinished) {
        this.canvas.parentNode.removeChild(this.canvas);
        this.$emit('complete');
        if (this.onComplete) this.onComplete();
      }
      this.isFinished = true;
    },
  },

  watch: {
    forceReveal(val) {
      if (val) this.reveal();
    },
  },

  mounted() {
    this.initCanvas();
    this.drawImage();
    this.prepBrush();
    if (typeof this.onComplete !== 'undefined') {
      // eslint-disable-next-line
      console.warn(
        '[vue-scratchcard] - `onComplete` call is deprecated in favor of `complete` event',
      );
    }
  },
};
</script>

<style scoped>
.scratchcard {
  position: relative;
  display: block;
}

.scratchcard > * {
  position: absolute;
  width: 100%;
  height: 100%;
  display: block;
}

.scratchcard-overlay {
  z-index: 1;
}
</style>
