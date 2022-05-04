<template>
  <div>
    <div class="sheet">
      <h2 class="title title--small sheet__title">Выберите тесто</h2>
      <div class="sheet__content dough">
        <RadioButton
          @changeRadio="changeDoughId"
          v-for="doughItem in dough"
          :key="doughItem.id"
          radioType="dough"
          :id="doughItem.id"
          :name="doughItem.name"
          :description="doughItem.description"
          :value="doughItem.value"
          :default-id="currentDoughId"
        />
      </div>
    </div>
  </div>
</template>

<script>
import RadioButton from "@/common/components/RadioButton.vue";

export default {
  name: "BuilderDoughSelector",
  components: { RadioButton },
  props: {
    dough: {
      type: Array,
      required: true,
    },
    currentDoughId: {
      type: Number,
      required: true,
    },
  },
  methods: {
    changeDoughId(doughId) {
      this.$emit("changeDough", doughId);
    },
  },
};
</script>

<style lang="scss">
.dough__input {
  position: relative;

  margin-right: 8%;
  margin-bottom: 20px;
  padding-left: 50px;

  cursor: pointer;

  b {
    @include r-s16-h19;

    &::before {
      @include p_center-v;

      width: 36px;
      height: 36px;

      content: "";
      transition: 0.3s;

      border-radius: 50%;
      background-repeat: no-repeat;
      background-position: center;
      background-size: cover;
    }
  }

  span {
    @include l-s11-h13;

    display: block;
  }

  &--light {
    b {
      &::before {
        background-image: url("~@/assets/img/dough-light.svg");
      }
    }
  }

  &--large {
    b {
      &::before {
        background-image: url("~@/assets/img/dough-large.svg");
      }
    }
  }

  &:hover {
    b::before {
      box-shadow: $shadow-regular;
    }
  }

  input {
    &:checked + b::before {
      box-shadow: $shadow-large;
    }
  }
}
</style>
