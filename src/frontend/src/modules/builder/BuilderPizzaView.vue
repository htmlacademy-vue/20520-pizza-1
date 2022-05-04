<template>
  <AppDrop @drop="$emit('drop', $event)">
    <div class="pizza" :class="foundationClass">
      <div class="pizza__wrapper">
        <div
          v-for="[key, value] of fillings"
          :key="key"
          :class="[
            `pizza__filling--${INGREDIENTS_MODIFIER[key]}`,
            { 'pizza__filling--second': value === TWO_INGREDIENTS },
            { 'pizza__filling--third': value === THREE_INGREDIENTS },
          ]"
          class="pizza__filling"
        />
      </div>
    </div>
  </AppDrop>
</template>

<script>
import AppDrop from "@/common/components/AppDrop";
import SAUCES_MODIFIER from "@/common/enums/sauces";
import INGREDIENTS_MODIFIER from "@/common/enums/ingredients";

const TWO_INGREDIENTS = 2;
const THREE_INGREDIENTS = 3;

export default {
  name: "BuilderPizzaView",
  components: { AppDrop },
  data() {
    return {
      DOUGH_MODIFIER: {
        1: "small",
        2: "big",
      },
      SAUCES_MODIFIER,
      INGREDIENTS_MODIFIER,
      TWO_INGREDIENTS,
      THREE_INGREDIENTS,
    };
  },
  props: {
    currentDoughId: {
      type: Number,
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
  computed: {
    foundationClass() {
      return `pizza--foundation--${this.DOUGH_MODIFIER[this.currentDoughId]}-${
        this.SAUCES_MODIFIER[this.currentSauceId]
      }`;
    },
    fillings() {
      return Object.entries(this.ingredientsCount).filter(([, value]) => value);
    },
  },
};
</script>

<style lang="scss">
.pizza {
  position: relative;

  display: block;

  box-sizing: border-box;
  width: 100%;

  background-repeat: no-repeat;
  background-position: center;
  background-size: 100%;

  &--foundation--big-creamy {
    background-image: url("~@/assets/img/foundation/big-creamy.svg");
  }

  &--foundation--big-tomato {
    background-image: url("~@/assets/img/foundation/big-tomato.svg");
  }

  &--foundation--small-creamy {
    background-image: url("~@/assets/img/foundation/small-creamy.svg");
  }

  &--foundation--small-tomato {
    background-image: url("~@/assets/img/foundation/small-tomato.svg");
  }
}

.pizza__wrapper {
  width: 100%;
  padding-bottom: 100%;
}

.pizza__filling {
  position: absolute;
  top: 0;
  left: 0;

  display: block;

  width: 100%;
  height: 100%;

  background-repeat: no-repeat;
  background-position: center;
  background-size: 100%;

  &--second {
    transform: rotate(45deg);
  }

  &--third {
    transform: rotate(-45deg);
  }

  &--ananas {
    background-image: url("~@/assets/img/filling-big/ananas.svg");
  }

  &--bacon {
    background-image: url("~@/assets/img/filling-big/bacon.svg");
  }

  &--blue_cheese {
    background-image: url("~@/assets/img/filling-big/blue_cheese.svg");
  }

  &--cheddar {
    background-image: url("~@/assets/img/filling-big/cheddar.svg");
  }

  &--chile {
    background-image: url("~@/assets/img/filling-big/chile.svg");
  }

  &--ham {
    background-image: url("~@/assets/img/filling-big/ham.svg");
  }

  &--jalapeno {
    background-image: url("~@/assets/img/filling-big/jalapeno.svg");
  }

  &--mozzarella {
    background-image: url("~@/assets/img/filling-big/mozzarella.svg");
  }

  &--mushrooms {
    background-image: url("~@/assets/img/filling-big/mushrooms.svg");
  }

  &--olives {
    background-image: url("~@/assets/img/filling-big/olives.svg");
  }

  &--onion {
    background-image: url("~@/assets/img/filling-big/onion.svg");
  }

  &--parmesan {
    background-image: url("~@/assets/img/filling-big/parmesan.svg");
  }

  &--salami {
    background-image: url("~@/assets/img/filling-big/salami.svg");
  }

  &--salmon {
    background-image: url("~@/assets/img/filling-big/salmon.svg");
  }

  &--tomatoes {
    background-image: url("~@/assets/img/filling-big/tomatoes.svg");
  }
}
</style>
