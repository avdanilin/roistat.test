<template>
  <fieldset :class="['form-input', itemClass]">
    <label :class="['form-input__label', labelClass]" :for="id">
      <span class="form-input__text">{{ label }}</span>
      <input
        :id="id"
        :name="name"
        :class="['form-input__item', inputClass]"
        :pattern="pattern"
        :type="type"
        :required="required"
        v-model="value"
      >
    </label>
  </fieldset>
</template>

<script>
export default {
  name: 'inputForm',
  props: {
    itemClass: {
      type: String,
      default: null
    },
    labelClass: {
      type: String,
      default: null
    },
    inputClass: {
      type: String,
      default: null
    },
    name: {
      type: String,
      default: null
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
  border: unset;
  margin: 0;
  padding: 0;
}

.form-input__label {
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
  width: 100%;
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

.form-input__text {
  flex-basis: 4.5rem;
  flex-shrink: 0;
  white-space: nowrap;
}
</style>
