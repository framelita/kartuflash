<template lang="pug">
.playing-cards
  .playing-cards__container(v-if="hasBeenShuffled")
    Card(
      v-for="(card, index) in allCards"
      :src="require('@/assets/images/' + card.img)"
      :en="card.en"
      :id="card.id"
      :is-active="index === activeIndex"
      :is-previous="index === previousIndex"
      :is-next="index === nextIndex"
    )
  .playing-cards__buttons
    button(
      type="button"
      @click="goToPreviousCard"
    ) Previous card
    button(
      type="button"
      @click="goToNextCard"
    ) Next card
</template>

<script>
import Fruits from '@/assets/data/fruits.json';

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
  },
  mounted() {
    this.combineCards();
    this.shuffleCards();
    console.log(this.allCards, Fruits);
  },
  methods: {
    combineCards() {
      this.allCards = [...Fruits];
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

.playing-cards__container {
  width: 100%;
  height: 400px;
  position: relative;
  overflow: hidden;
}

.playing-cards__buttons {
  button {
    padding: 10px 20px;
    margin: 0 5px;
    cursor: pointer;
  }
}
</style>
