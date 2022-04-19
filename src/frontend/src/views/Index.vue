<template>
  <div>
    <header class="header">
      <div class="header__logo">
        <a class="logo">
          <img
            src="@/assets/img/logo.svg"
            alt="V!U!E! Pizza logo"
            width="90"
            height="40"
          />
        </a>
      </div>
      <div class="header__cart">
        <a>0 ₽</a>
      </div>
      <div class="header__user">
        <a href="#" class="header__login"><span>Войти</span></a>
      </div>
    </header>

    <main class="content">
      <form action="#" method="post">
        <div class="content__wrapper">
          <h1 class="title title--big">Конструктор пиццы</h1>

          <div class="content__dough">
            <div class="sheet">
              <h2 class="title title--small sheet__title">Выберите тесто</h2>
              <div class="sheet__content dough">
                <label
                  v-for="doughItem in dough"
                  :key="doughItem.id"
                  class="dough__input"
                  :class="`dough__input--${doughItem.value}`"
                >
                  <input
                    type="radio"
                    name="dought"
                    :value="doughItem.value"
                    class="visually-hidden"
                    :checked="doughItem.id === DEFAULT_DOUGH_ID"
                  />
                  <b>{{ doughItem.name }}</b>
                  <span>{{ doughItem.description }}</span>
                </label>
              </div>
            </div>
          </div>

          <div class="content__diameter">
            <div class="sheet">
              <h2 class="title title--small sheet__title">Выберите размер</h2>
              <div class="sheet__content diameter">
                <label
                  v-for="size in sizes"
                  :key="size.id"
                  class="diameter__input"
                  :class="`diameter__input--${size.value}`"
                >
                  <input
                    type="radio"
                    name="diameter"
                    :value="size.value"
                    class="visually-hidden"
                    :checked="size.id === DEFAULT_SIZE_ID"
                  />
                  <span>{{ size.name }}</span>
                </label>
              </div>
            </div>
          </div>

          <div class="content__ingredients">
            <div class="sheet">
              <h2 class="title title--small sheet__title">
                Выберите ингредиенты
              </h2>
              <div class="sheet__content ingredients">
                <div class="ingredients__sauce">
                  <p>Основной соус:</p>

                  <label
                    v-for="sauce in sauces"
                    :key="sauce.id"
                    class="radio ingredients__input"
                  >
                    <input
                      type="radio"
                      name="sauce"
                      :value="sauce.value"
                      :checked="sauce.id === DEFAULT_SAUCE_ID"
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
                      <span
                        class="filling"
                        :class="`filling--${ingredient.value}`"
                      >
                        {{ ingredient.name }}
                      </span>

                      <div class="counter counter--orange ingredients__counter">
                        <button
                          type="button"
                          class="counter__button counter__button--minus"
                          disabled
                        >
                          <span class="visually-hidden">Меньше</span>
                        </button>
                        <input
                          type="text"
                          name="counter"
                          class="counter__input"
                          value="0"
                        />
                        <button
                          type="button"
                          class="counter__button counter__button--plus"
                        >
                          <span class="visually-hidden">Больше</span>
                        </button>
                      </div>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>

          <div class="content__pizza">
            <label class="input">
              <span class="visually-hidden">Название пиццы</span>
              <input
                type="text"
                name="pizza_name"
                placeholder="Введите название пиццы"
              />
            </label>

            <div class="content__constructor">
              <div class="pizza pizza--foundation--big-tomato">
                <div class="pizza__wrapper">
                  <div class="pizza__filling pizza__filling--ananas"></div>
                  <div class="pizza__filling pizza__filling--bacon"></div>
                  <div class="pizza__filling pizza__filling--cheddar"></div>
                </div>
              </div>
            </div>

            <div class="content__result">
              <p>Итого: 0 ₽</p>
              <button type="button" class="button" disabled>Готовьте!</button>
            </div>
          </div>
        </div>
      </form>
    </main>
  </div>
</template>

<script>
import pizza from "@/static/pizza.json";
import {
  normalizeDough,
  normalizeSize,
  normalizeIngredients,
  normalizeSauces,
} from "@/common/helpers";
import {
  DEFAULT_DOUGH_ID,
  DEFAULT_SIZE_ID,
  DEFAULT_SAUCE_ID,
} from "@/common/constants";

const { dough, ingredients, sauces, sizes } = pizza;

export default {
  name: "Index.vue",
  data() {
    return {
      dough: dough.map((doughItem) => normalizeDough(doughItem)),
      ingredients: ingredients.map((ingredient) =>
        normalizeIngredients(ingredient)
      ),
      sauces: sauces.map((sauce) => normalizeSauces(sauce)),
      sizes: sizes.map((size) => normalizeSize(size)),
      DEFAULT_DOUGH_ID,
      DEFAULT_SIZE_ID,
      DEFAULT_SAUCE_ID,
    };
  },
};
</script>

<style lang="scss" scoped>
.header__login {
  &::after {
    display: inline-block;

    width: 32px;
    height: 32px;
    margin-left: 8px;

    content: "";
    vertical-align: middle;

    background: url("~@/assets/img/login.svg") no-repeat center;
    background-size: auto 50%;
  }
}
</style>
