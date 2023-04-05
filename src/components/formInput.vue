<template>
  <label :class="['form-input', labelClass]" :for="id">{{ label }}
    <input
      :id="id"
      :class="['form-input__item', inputClass]"
      :pattern="pattern"
      :type="type"
      :required="required"
      v-model="value"
    >
  </label>
</template>

<script>
export default {
  name: 'inputForm',
  props: {
    labelClass: {
      type: String,
      default: 'form-input'
    },
    inputClass: {
      type: String,
      default: 'form-input__item'
    },
    id: {
      type: String,
      default: 'name'
    },
    label: {
      type: String,
      default: 'Имя'
    },
    type: {
      type: String,
      default: 'text'
    },
    pattern: {
      type: String,
      default: null
    },
    required: {
      type: Boolean,
      default: false
    }
  },
  data: () => ({
    value: ''
  })
}
</script>

<style scoped>
.form-input {
  font-family: 'Roboto', Arial, sans-serif;
  font-size: .8rem;
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  gap: 0 0.5rem;
  cursor: pointer;
}

.form-input__item {
  font-size: .75rem;
  padding: .25rem .1rem;
  border-top: 0;
  border-right: 0;
  border-left: 0;
  border-bottom: 1px solid black;
  appearance: none;
  -moz-appearance: textfield;
}

.form-input__item::-webkit-outer-spin-button,
.form-input__item::-webkit-inner-spin-button {
  appearance: none;
  -moz-appearance: textfield;
}

.form-input__item:focus-within,
.form-input__item:focus {
  outline: 1px solid;
  border-radius: 4px;
}
</style>
