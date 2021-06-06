<template lang="pug">
.settings(
  :class="{ 'is-active': isActive }"
)
  button.settings__button(
    type="button"
    @click="toggleSettings"
    title="Settings"
  )
    i.icon(
      :class="settingsIcon"
    )
  .settings__menu
    .settings__title Category
    .settings__options
      .settings__checkbox(
        v-for="(category, index) in temporaryCategories"
      )
        input(
          type="checkbox"
          :id="category.value"
          :checked="category.isActive"
          @change="changeCategory(index)"
        )
        label(
          :for="category.value"
        ) {{ category.name }}
    .settings__save
      button.btn(
        type="button"
        @click="saveSettings"
      ) Save settings
</template>

<script>
export default {
  data() {
    return {
      temporaryCategories: [],
      isActive: false,
    };
  },
  props: {
    categories: { type: Array, default: () => [] },
  },
  computed: {
    settingsIcon() {
      return this.isActive ? 'icon-close' : 'icon-settings';
    },
  },
  mounted() {
    this.temporaryCategories = [...this.categories];
  },
  methods: {
    toggleSettings() {
      this.isActive = !this.isActive;
    },
    changeCategory(index) {
      this.temporaryCategories[index].isActive = !this.temporaryCategories[index].isActive;
    },
    saveSettings() {
      this.$emit('update-category', this.temporaryCategories);
    },
  },
};
</script>

<style lang="scss">
.settings {
  display: inline-block;

  &.is-active {
    .settings__menu {
      opacity: 1;
      visibility: visible;
      transform: translate(0, 0);
    }
  }
}

.settings__button {
  cursor: pointer;
  position: absolute;
  top: 20px;
  right: 20px;
  width: 36px;
  height: 36px;
  font-size: 1.25rem;
  background: transparent;
  padding: 0;
  border: 0;
}

.settings__menu {
  position: absolute;
  background: #fff;
  padding: 40px;
  top: 70px;
  width: 100%;
  max-width: 420px;
  right: 0;
  text-align: left;
  opacity: 0;
  visibility: hidden;
  transform: translate(0, 20px);
  transition: transform 0.3s cubic-bezier(0, 0, 0, 1), opacity 0.3s cubic-bezier(0, 0, 0, 1), visibility 0.3s cubic-bezier(0, 0, 0, 1);
}

.settings__title {
  font-weight: bold;
  font-size: 1.5em;
}

.settings__options {
  text-align: left;
  margin: 0.5em 0 1em;

  .settings__checkbox {
    line-height: 1.4;
    margin: 5px 0;

    input {
      margin-right: 5px;
    }
  }
}

.settings__save {
  button {
  }
}
</style>
