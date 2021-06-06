<template lang="pug">
.playing-cards
  img.playing-cards__preloader(
    v-if="isLoading"
    src="@/assets/images/preloader.svg"
  )
  template(v-else)
    .playing-cards__container(
      v-if="hasBeenShuffled"
    )
      Card(
        v-for="(card, index) in allCards"
        :key="card.en"
        :src="require('@/assets/images/' + card.img)"
        :en="card.en"
        :id="card.id"
        :is-active="index === activeIndex"
        :is-previous="index === previousIndex"
        :is-next="index === nextIndex"
        @next-card="goToNextCard"
        @previous-card="goToPreviousCard"
      )
    .playing-cards__buttons
      button.btn.btn-shuffle(
        type="button"
        title="Shuffle cards"
        @click="reshuffleCards"
      )
        i.icon.icon-shuffle
      button.btn(
        type="button"
        title="Previous card"
        @click="goToPreviousCard"
      )
        i.icon.icon-chevron-left
      button.btn(
        type="button"
        title="Next card"
        @click="goToNextCard"
      )
        i.icon.icon-chevron-right
</template>

<script>
export default {
  data() {
    return {
      allCards: [],
      hasBeenShuffled: false,
      activeIndex: 0,
      swipePosition: {
        x: 0,
      },
    };
  },
  props: {
    categories: { type: Array, default: () => [] },
    isLoading: { type: Boolean, default: false },
  },
  computed: {
    previousIndex() {
      let index = this.activeIndex - 1;
      if (index < 0) index = this.allCards.length - 1;
      return index;
    },
    nextIndex() {
      let index = this.activeIndex + 1;
      if (index >= this.allCards.length) index = 0;
      return index;
    },
    activeCategories() {
      return this.categories.filter(cat => cat.isActive);
    },
  },
  mounted() {
    this.combineActiveCards();
    this.shuffleCards();
  },
  watch: {
    isLoading(val) {
      if (!val) this.reshuffleCards();
    },
  },
  methods: {
    touchHandler() {
      console.log('touch')
    },
    combineActiveCards() {
      console.log('activeCategories', this.activeCategories.length)
      const cards = this.activeCategories.reduce((arr, category) => {
        if (category && category.cards) {
          arr = arr.concat(category.cards);
        }
        console.log('arr', arr.length)
        return arr;
      }, []);
      this.allCards = cards;
    },
    shuffleCards() {
      for (let i = this.allCards.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [this.allCards[i], this.allCards[j]] = [this.allCards[j], this.allCards[i]];
      }
      this.hasBeenShuffled = true;
    },
    reshuffleCards() {
      this.hasBeenShuffled = false;
      this.shuffleCards();
    },
    goToPreviousCard() {
      this.activeIndex = this.previousIndex;
    },
    goToNextCard() {
      this.activeIndex = this.nextIndex;
    },
    setDragging(event) {
      console.log('setDragging', event.detail);
      this.swipePosition.x = event.detail.x;
    },
    checkDragging(event) {
      const distance = event.detail.x - this.swipePosition.x;
      console.log('checkDragging', event.detail, distance);
    }
  },
};
</script>

<style lang="scss">
.playing-cards {
  width: 100%;
  max-width: 360px;
  overflow: hidden;
  position: relative;
  display: block;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  align-content: center;
  flex-direction: column;
}

.playing-cards__preloader {
  width: 64px;
}

.playing-cards__container {
  width: 100%;
  height: 400px;
  position: relative;
  overflow: hidden;
}

.playing-cards__buttons {
  button {
    padding: 0;
    width: 36px;
    height: 36px;
    font-size: 1.25rem;
    margin: 0 5px;
    cursor: pointer;
  }
}
</style>
