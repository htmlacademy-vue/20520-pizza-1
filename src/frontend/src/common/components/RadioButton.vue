<template>
  <label :class="`${name}__input ${name}__input--${data.value}`">
    <input
      type="radio"
      :name="`${name}`"
      :value="data.value"
      class="visually-hidden"
      :checked="data.id === defaultId"
    />
    <b>
      <slot name="name" />
    </b>
    <span>
      <slot name="description" />
    </span>
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
};
</script>
