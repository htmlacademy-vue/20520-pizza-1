<template>
  <label :class="`${radioType}__input ${radioType}__input--${value}`">
    <input
      @change="changeValue"
      type="radio"
      :name="radioType"
      :value="value"
      class="visually-hidden"
      :checked="id === defaultId"
    />
    <b v-if="name">{{ name }}</b>
    <span v-if="description">{{ description }}</span>
  </label>
</template>

<script>
export default {
  name: "RadioButton",
  props: {
    radioType: {
      type: String,
      required: true,
    },
    name: {
      type: String,
      default: "",
    },
    description: {
      type: String,
      default: "",
    },
    value: {
      type: String,
      required: true,
    },
    id: {
      type: Number,
      required: true,
    },
    defaultId: {
      type: Number,
      default: -1,
    },
  },
  methods: {
    changeValue: function () {
      this.$emit("changeRadio", this.id);
    },
  },
};
</script>
