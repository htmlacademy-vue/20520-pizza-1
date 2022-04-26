<template>
  <label :class="`${name}__input ${name}__input--${data.value}`">
    <input
      @change="changeValue"
      type="radio"
      :name="`${name}`"
      :value="data.value"
      class="visually-hidden"
      :checked="data.id === defaultId"
    />
    <b v-if="data.name">{{ data.name }}</b>
    <span v-if="data.description">{{ data.description }}</span>
  </label>
</template>

<script>
export default {
  name: "RadioButton",
  props: {
    name: {
      type: String,
      required: true,
    },
    data: {
      type: Object,
      required: true,
    },
    defaultId: {
      type: Number,
      default: -1,
    },
  },
  methods: {
    changeValue: function () {
      this.$emit("changeRadio", this.data.id);
    },
  },
};
</script>
