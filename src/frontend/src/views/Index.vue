<template>
  <div>
    <AppLayoutHeader :pizza-price="pizzaPrice" />

    <main class="content">
      <form action="#" method="post">
        <div class="content__wrapper">
          <h1 class="title title--big">Конструктор пиццы</h1>

          <BuilderDoughSelector
            @changeDough="changeDough"
            class="content__dough"
            :dough="pizzaData.dough"
            :current-dough-id="pizzaSettings.currentDoughId"
          />
          <BuilderSizeSelector
            @changeSize="changeSize"
            class="content__diameter"
            :sizes="pizzaData.sizes"
            :current-size-id="pizzaSettings.currentSizeId"
          />
          <BuilderIngredientsSelector
            @changeIngredientsCount="changeIngredientsCount"
            @changeSauce="changeSauce"
            class="content__ingredients"
            :ingredients="pizzaData.ingredients"
            :sauces="pizzaData.sauces"
            :current-sauce-id="pizzaSettings.currentSauceId"
            :ingredients-count="pizzaSettings.ingredientsCount"
          />

          <div class="content__pizza">
            <label class="input">
              <span class="visually-hidden">Название пиццы</span>
              <input
                type="text"
                name="pizza_name"
                placeholder="Введите название пиццы"
              />
            </label>
            <BuilderPizzaView
              @drop="onDropIngredient"
              :pizza-settings="pizzaSettings"
              class="content__constructor"
            />
            <BuilderPriceCounter
              @changePrice="changePrice"
              :pizza-data="pizzaData"
              :pizza-settings="pizzaSettings"
              class="content__result"
            />
          </div>
        </div>
      </form>
    </main>
  </div>
</template>

<script>
import BuilderDoughSelector from "@/modules/builder/BuilderDoughSelector.vue";
import BuilderSizeSelector from "@/modules/builder/BuilderSizeSelector.vue";
import BuilderIngredientsSelector from "@/modules/builder/BuilderIngredientsSelector.vue";
import BuilderPizzaView from "@/modules/builder/BuilderPizzaView.vue";
import BuilderPriceCounter from "@/modules/builder/BuilderPriceCounter.vue";
import pizza from "@/static/pizza.json";
import {
  normalizeDough,
  normalizeSize,
  normalizeIngredients,
  normalizeSauces,
} from "@/common/helpers";
import {
  DEFAULT_DOUGH_ID,
  DEFAULT_SAUCE_ID,
  DEFAULT_SIZE_ID,
} from "@/common/constants";
import AppLayoutHeader from "@/layouts/AppLayoutHeader";

const { dough, ingredients, sauces, sizes } = pizza;
const normalizedIngredients = ingredients.map((ingredient) =>
  normalizeIngredients(ingredient)
);

export default {
  name: "Index.vue",
  components: {
    AppLayoutHeader,
    BuilderSizeSelector,
    BuilderDoughSelector,
    BuilderIngredientsSelector,
    BuilderPizzaView,
    BuilderPriceCounter,
  },
  data() {
    return {
      pizzaData: {
        dough: dough.map((doughItem) => normalizeDough(doughItem)),
        ingredients: normalizedIngredients,
        sauces: sauces.map((sauce) => normalizeSauces(sauce)),
        sizes: sizes.map((size) => normalizeSize(size)),
      },
      pizzaSettings: {
        currentDoughId: DEFAULT_DOUGH_ID,
        currentSizeId: DEFAULT_SIZE_ID,
        currentSauceId: DEFAULT_SAUCE_ID,
        ingredientsCount: Object.fromEntries(
          normalizedIngredients.map((ingredient) => [ingredient.id, 0])
        ),
      },
      pizzaPrice: 0,
    };
  },
  methods: {
    changePrice(newPrice) {
      this.pizzaPrice = newPrice;
    },
    changeDough(newDoughId) {
      this.pizzaSettings.currentDoughId = newDoughId;
    },
    changeSize(newSizeId) {
      this.pizzaSettings.currentSizeId = newSizeId;
    },
    changeIngredientsCount(newIngredients) {
      this.pizzaSettings.ingredientsCount = newIngredients;
    },
    changeSauce(newSauceId) {
      this.pizzaSettings.currentSauceId = newSauceId;
    },
    onDropIngredient(ingredient) {
      this.pizzaSettings.ingredientsCount[ingredient.id]++;
    },
  },
};
</script>
