import backgroundUrl from '~/assets/images/1.png';
<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${
        (heightBrowser - 16 * 4) / Math.sqrt(cardsContext.length) - 16
      }px`,
      width: `${
        (((heightBrowser - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
        4
      }px`,
      perspective: `${
        ((((heightBrowser - 16 * 4) / Math.sqrt(cardsContext.length) - 16) *
          3) /
          4) *
        2
      }px`,
    }"
  >
    <div
      class="card__inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="() => onToggleFlipCard()"
    >
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="{
            backgroundSize: `${
              (((heightBrowser - 16 * 4) / Math.sqrt(cardsContext.length) -
                16) *
                3) /
              4 /
              3
            }px ${
              (((heightBrowser - 16 * 4) / Math.sqrt(cardsContext.length) -
                16) *
                3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{ backgroundImage: `url(${urlAsset}/${imgBackFaceUrl})` }"
        ></div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    imgBackFaceUrl: {
      type: String,
      require: true,
    },
    card: {
      type: [String, Number, Object],
      require: true,
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      urlAsset: new URL("../assets", import.meta.url).href,
      isFlipped: false,
      isDisabled: false,
      heightBrowser: window.innerHeight,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled) return false;

      this.isFlipped = !this.isFlipped;
      if (this.isFlipped == true) {
        this.$emit("onFlip", this.card);
      }
    },

    onFlipBackCard() {
      this.isFlipped = false;
    },

    onEnabledDisableMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 0.9s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card__face--front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center;
  height: 100%;
  widows: 100%;
}

.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face--back .card__content {
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}

.card.disabled .card__inner {
  cursor: default;
}
</style>
