<template>
  <div>
    <div class="sheet">
      <h2 class="title title--small sheet__title">Выберите размер</h2>
      <div class="sheet__content diameter">
        <RadioButton
          @changeRadio="changeSizeId"
          v-for="size in sizes"
          :key="size.id"
          :data="size"
          :default-id="currentSizeId"
          name="diameter"
        />
      </div>
    </div>
  </div>
</template>

<script>
import RadioButton from "@/common/components/RadioButton.vue";

export default {
  name: "BuilderSizeSelector",
  components: { RadioButton },
  props: {
    sizes: {
      type: Array,
      required: true,
    },
    currentSizeId: {
      type: Number,
      required: true,
    },
  },
  methods: {
    changeSizeId(sizeId) {
      this.$emit("changeSize", sizeId);
    },
  },
};
</script>

<style lang="scss">
.diameter__input {
  margin-right: 8.7%;
  margin-bottom: 20px;
  padding-top: 7px;
  padding-bottom: 6px;

  cursor: pointer;

  b {
    @include r-s16-h19;

    position: relative;

    padding-left: 46px;

    &::before {
      @include p_center_v;

      width: 36px;
      height: 36px;

      content: "";
      transition: 0.3s;

      border-radius: 50%;
      background-color: $green-100;
      background-image: url("~@/assets/img/diameter.svg");
      background-repeat: no-repeat;
      background-position: center;
    }
  }

  &:nth-child(3n) {
    margin-right: 0;
  }

  &--small {
    b::before {
      background-size: 18px;
    }
  }

  &--normal {
    b::before {
      background-size: 29px;
    }
  }

  &--big {
    b::before {
      background-size: 100%;
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
