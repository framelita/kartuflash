<template lang="pug">
button.card(
  type="button"
  :class="cssClass"
  v-touch:tap="flipCard"
  v-touch:swipe.left="goToNextCard"
  v-touch:swipe.right="goToPreviousCard"
)
  .card__front
    .card__image(v-if="src")
      img(:src="src")
    .card__main {{ en }}
  .card__back
    .card__image(v-if="src")
      img(:src="src")
    .card__main {{ id }}
    .card__secondary {{ en }}
</template>

<script>
export default {
  props: {
    src: { type: String, default: '' },
    id: { type: String, default: '' },
    en: { type: String, default: '' },
    isActive: { type: Boolean, default: false },
    isPrevious: { type: Boolean, default: false },
    isNext: { type: Boolean, default: false },
  },
  data() {
    return {
      isFlipped: false,
    };
  },
  computed: {
    cssClass() {
      const cssClass = [];

      if (this.isFlipped) cssClass.push('is-flipped');
      if (this.isActive) cssClass.push('is-active');
      if (this.isPrevious) cssClass.push('is-previous');
      if (this.isNext) cssClass.push('is-next');

      return cssClass;
    },
  },
  watch: {
    isActive(val) {
      if (!val) {
        this.isFlipped = false;
      }
    },
  },
  methods: {
    flipCard() {
      console.log('flip')
      this.isFlipped = !this.isFlipped;
    },
    goToPreviousCard() {
      console.log('goToPreviousCard')
      this.$emit('previous-card');
    },
    goToNextCard() {
      console.log('goToNextCard')
      this.$emit('next-card');
    },
  },
};
</script>

<style lang="scss">
.card {
  width: 240px;
  height: 300px;
  position: absolute;
  background: transparent;
  perspective: 1000px;
  border-radius: 10px;
  background: #eee;
  border: 4px solid #ddd;
  transition: transform 0.3s ease-in-out;
  transform-style: preserve-3d;
  transform-origin: center;
  padding: 20px;
  cursor: pointer;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
  user-select: none;

  &:not(.is-previous):not(.is-next):not(.is-active) {
    display: none;
  }

  &.is-flipped {
    transform: rotateY(180deg);
  }

  &.is-previous {
    transform: translateX(-200%);
  }

  &.is-next {
    transform: translateX(200%);
  }
}

.card__front,
.card__back {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #eee;
  backface-visibility: hidden;
  display: flex;
  align-content: center;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  border-radius: 10px;
}

.card__back {
  transform: rotateY(180deg);
}

.card__main {
  font-size: 2.25em;
  font-weight: bold;
}

.card__secondary {
  margin-top: 0.25em;
  font-size: 1.5em;
}

.card__main,
.card__secondary {
  text-transform: capitalize;
}

.card__image {
  margin-bottom: 1em;

  img {
    width: auto;
    display: block;
    height: 120px;
  }
}
</style>
