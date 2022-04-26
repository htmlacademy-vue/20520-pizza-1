<template>
  <div>
    <div class="sheet">
      <h2 class="title title--small sheet__title">Выберите ингредиенты</h2>
      <div class="sheet__content ingredients">
        <div class="ingredients__sauce">
          <p>Основной соус:</p>

          <label
            v-for="sauce in sauces"
            :key="sauce.id"
            class="radio ingredients__input"
          >
            <input
              @change="changeSauce(sauce.id)"
              type="radio"
              name="sauce"
              :value="sauce.value"
              :checked="sauce.id === currentSauceId"
            />
            <span>{{ sauce.name }}</span>
          </label>
        </div>

        <div class="ingredients__filling">
          <p>Начинка:</p>

          <ul class="ingredients__list">
            <li
              v-for="ingredient in ingredients"
              :key="ingredient.id"
              class="ingredients__item"
            >
              <span class="filling" :class="`filling--${ingredient.value}`">
                {{ ingredient.name }}
              </span>
              <ItemCounter
                @changeCount="changeIngredientsCount($event, ingredient.id)"
                class="ingredients__counter"
              />
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ItemCounter from "@/common/components/ItemCounter.vue";
export default {
  name: "BuilderIngredientsSelector",
  components: { ItemCounter },
  props: {
    sauces: {
      type: Array,
      required: true,
    },
    ingredients: {
      type: Array,
      required: true,
    },
    currentSauceId: {
      type: Number,
      required: true,
    },
    ingredientsCount: {
      type: Object,
      required: true,
    },
  },
  methods: {
    changeIngredientsCount(newCount, ingredientId) {
      this.ingredientsCount[ingredientId] = newCount;
      this.$emit("changeIngredientsCount", this.ingredientsCount);
    },
    changeSauce: function (sauceId) {
      this.$emit("changeSauce", sauceId);
    },
  },
};
</script>

<style lang="scss">
.ingredients__sauce {
  display: flex;
  align-items: center;
  flex-wrap: wrap;

  width: 100%;
  margin-bottom: 14px;

  p {
    @include r-s16-h19;

    margin-top: 0;
    margin-right: 16px;
    margin-bottom: 10px;
  }
}

.ingredients__input {
  margin-right: 24px;
  margin-bottom: 10px;
}

.ingredients__filling {
  width: 100%;

  p {
    @include r-s16-h19;

    margin-top: 0;
    margin-bottom: 16px;
  }
}

.ingredients__list {
  @include clear-list;

  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
}

.ingredients__item {
  width: 100px;
  min-height: 40px;
  margin-right: 17px;
  margin-bottom: 35px;
}

.ingredients__counter {
  width: 54px;
  margin-top: 10px;
  margin-left: 36px;
}
</style>
