<template>
  <select v-model="modelValue" v-on:change="changeOption">
    <option disabled value="">Выберите из списка</option>
    <option
      v-for="option in options"
      v-bind:key="option.value"
      v-bind:value="option.value"
    >
      {{ option.name }}
    </option>
  </select>
</template>

<script>
export default {
  name: "my-select",
  props: {
    modelValue: {
      type: String,
    },
    options: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
      changeOption(event) {
          this.$emit('update:modelValue', event.target.value)
      }
  }
};
</script>

<style scoped></style>
