<template>
  <div>
    <p>Итого: {{ pizzaPrice }} ₽</p>
    <button
      type="button"
      class="button"
      :disabled="!Boolean(ingredientsCost) || !pizzaName"
    >
      Готовьте!
    </button>
  </div>
</template>

<script>
export default {
  name: "BuilderPriceCounter",
  data() {
    return {
      isSelectedIngredients: false,
    };
  },
  props: {
    pizzaName: {
      type: String,
      required: true,
    },
    dough: {
      type: Array,
      required: true,
    },
    sauces: {
      type: Array,
      required: true,
    },
    ingredients: {
      type: Array,
      required: true,
    },
    sizes: {
      type: Array,
      required: true,
    },
    currentDoughId: {
      type: Number,
      required: true,
    },
    currentSauceId: {
      type: Number,
      required: true,
    },
    currentSizeId: {
      type: Number,
      required: true,
    },
    ingredientsCount: {
      type: Object,
      required: true,
    },
  },
  computed: {
    ingredientsCost() {
      const ingredientsPrices = Object.fromEntries(
        this.ingredients.map((ingredient) => [ingredient.id, ingredient.price])
      );
      return Object.keys(this.ingredientsCount).reduce((sum, ingredientId) => {
        return (
          sum +
          this.ingredientsCount[ingredientId] * ingredientsPrices[ingredientId]
        );
      }, 0);
    },
    pizzaPrice() {
      const doughPrices = Object.fromEntries(
        this.dough.map((dough) => [dough.id, dough.price])
      );
      const saucesPrices = Object.fromEntries(
        this.sauces.map((sauce) => [sauce.id, sauce.price])
      );
      const sizesMultipliers = Object.fromEntries(
        this.sizes.map((size) => [size.id, size.multiplier])
      );
      return Number(
        (doughPrices[this.currentDoughId] +
          saucesPrices[this.currentSauceId] +
          this.ingredientsCost) *
          sizesMultipliers[this.currentSizeId]
      );
    },
  },
};
</script>
