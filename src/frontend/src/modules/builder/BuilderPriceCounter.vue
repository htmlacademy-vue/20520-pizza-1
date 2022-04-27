<template>
  <div>
    <p>Итого: {{ pizzaPrice }} ₽</p>
    <button type="button" class="button" :disabled="!pizzaPrice">
      Готовьте!
    </button>
  </div>
</template>

<script>
export default {
  name: "BuilderPriceCounter",
  props: {
    pizzaData: {
      type: Object,
      required: true,
    },
    pizzaSettings: {
      type: Object,
      required: true,
    },
  },
  computed: {
    pizzaPrice() {
      const doughPrices = Object.fromEntries(
        this.pizzaData.dough.map((dough) => [dough.id, dough.price])
      );
      const saucesPrices = Object.fromEntries(
        this.pizzaData.sauces.map((sauce) => [sauce.id, sauce.price])
      );
      const ingredientsPrices = Object.fromEntries(
        this.pizzaData.ingredients.map((ingredient) => [
          ingredient.id,
          ingredient.price,
        ])
      );
      const sizesMultipliers = Object.fromEntries(
        this.pizzaData.sizes.map((size) => [size.id, size.multiplier])
      );
      const ingredientsCost = Object.keys(
        this.pizzaSettings.ingredientsCount
      ).reduce((sum, ingredientId) => {
        return (
          sum +
          this.pizzaSettings.ingredientsCount[ingredientId] *
            ingredientsPrices[ingredientId]
        );
      }, 0);
      const totalPrice = Number(
        (doughPrices[this.pizzaSettings.currentDoughId] +
          saucesPrices[this.pizzaSettings.currentSauceId] +
          ingredientsCost) *
          sizesMultipliers[this.pizzaSettings.currentSizeId]
      );
      this.$emit("changePrice", totalPrice);
      return totalPrice;
    },
  },
};
</script>
