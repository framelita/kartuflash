<template lang="pug">
div
  header
    .logo
      img(src="@/assets/images/logo.svg")
    Settings(
      :categories="categories"
      @update-category="updateCategories"
    )
  main
    .container
      PlayingCards(
        :is-loading="isUpdating"
        :categories="categories"
      )
</template>

<script>
import Fruits from '@/assets/data/fruits.json';
import FoodsDrinks from '@/assets/data/foods-drinks.json';

export default {
  data() {
    return {
      categories: [
        {
          name: 'Fruit',
          value: 'fruit',
          isActive: true,
          cards: Fruits,
        },
        {
          name: 'Food & Drink',
          value: 'foods-drinks',
          isActive: true,
          cards: FoodsDrinks,
        },
      ],
      isUpdating: false,
    };
  },
  methods: {
    updateCategories(categories) {
      this.isUpdating = true;
      categories.forEach((category, index) => {
        this.categories[index].isActive = category.isActive;
      });
      setTimeout(() => {
        this.isUpdating = false;
      }, 300);
    },
  }
};
</script>

<style lang="scss">
.container {
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

header {
  text-align: center;
  padding: 1em;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  display: flex;
  justify-content: space-between;
  z-index: 10;
}

.logo {
  width: 150px;
  margin: 0 auto;
  display: inline-block;

  img {
    max-width: 100%;
  }

  @media screen and (min-width: 768px) {
    width: 200px;
  }
}

main {
  min-height: 100vh;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.btn {
  padding: 10px 20px;
  font-family: 'Karla', sans-serif;
  font-weight: bold;
  font-size: 0.875rem;
  background-color: #A3E261;
  border: 0;
  border-radius: 4px;
  color: #fff;
  transition: background-color 0.3s cubic-bezier(0, 0, 0, 1);

  &:hover {
    background-color: darken(#A3E261, 10%);
  }
}
</style>
