<template>
  <div>
    <AppLayoutHeader />

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
            @changeSauce="changeSauce"
            :ingredients="pizzaData.ingredients"
            :sauces="pizzaData.sauces"
            :current-sauce-id="pizzaSettings.currentSauceId"
            :ingredients-count.sync="pizzaSettings.ingredientsCount"
            class="content__ingredients"
          />

          <div class="content__pizza">
            <label class="input">
              <span class="visually-hidden">Название пиццы</span>
              <input
                v-model="pizzaName"
                type="text"
                name="pizza_name"
                placeholder="Введите название пиццы"
              />
            </label>
            <BuilderPizzaView
              @drop="onDropIngredient"
              :current-dough-id="pizzaSettings.currentDoughId"
              :current-sauce-id="pizzaSettings.currentSauceId"
              :ingredients-count="pizzaSettings.ingredientsCount"
              class="content__constructor"
            />
            <BuilderPriceCounter
              :pizzaName="pizzaName"
              :dough="pizzaData.dough"
              :ingredients="pizzaData.ingredients"
              :sauces="pizzaData.sauces"
              :sizes="pizzaData.sizes"
              :current-dough-id="pizzaSettings.currentDoughId"
              :current-size-id="pizzaSettings.currentSizeId"
              :current-sauce-id="pizzaSettings.currentSauceId"
              :ingredients-count="pizzaSettings.ingredientsCount"
              class="content__result"
            />
          </div>
        </div>
      </form>
    </main>
  </div>
</template>

<script>
import AppLayoutHeader from "@/layouts/AppLayoutHeader";
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
  MAX_INGREDIENT_COUNT,
} from "@/common/constants";

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
      pizzaName: "",
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
    };
  },
  methods: {
    changeDough(newDoughId) {
      this.pizzaSettings.currentDoughId = newDoughId;
    },
    changeSize(newSizeId) {
      this.pizzaSettings.currentSizeId = newSizeId;
    },
    changeSauce(newSauceId) {
      this.pizzaSettings.currentSauceId = newSauceId;
    },
    onDropIngredient(ingredient) {
      if (
        this.pizzaSettings.ingredientsCount[ingredient.id] <
        MAX_INGREDIENT_COUNT
      ) {
        this.pizzaSettings.ingredientsCount[ingredient.id]++;
      }
    },
  },
};
</script>
