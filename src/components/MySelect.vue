<template>
  <select v-model="modelValue" @change="changeOption">
    <option value="">{{ name }}</option>
    <option v-for="option in options" :key="option.value" :value="option.value">
      {{ option.name }}
    </option>
  </select>
</template>

<script>
export default {
  name: 'my-select',
  props: {
    modelValue: {
      type: String,
    },
    options: {
      type: Array,
      default: () => [],
    },
    name: {
      type: String,
    },
  },
  methods: {
    changeOption(event) {
      this.$emit('update:modelValue', event.target.value);
    },
  },
};
</script>

<style>
select {
  font-size: 15px;
  padding: 5px;
  width: 200px;
  margin-right: 10px;
}
</style>
