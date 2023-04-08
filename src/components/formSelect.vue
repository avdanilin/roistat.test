<template>
  <fieldset :class="['form-select', itemClass]">
    <label :class="['form-select__label', labelClass]" :for="id">
      <span class="form-select__text">{{ label }}</span>
      <select
        :id="id"
        :name="name"
        :class="['form-select__item', selectClass]"
        :required="required"
        v-model="current"
      >
        <option v-for="(option, ix) in arrayItems" :key="ix" value="option.name">{{ option.name }}</option>
      </select>
    </label>
  </fieldset>
</template>

<script>
export default {
  name: 'formSelect',
  props: {
    itemClass: {
      type: String,
      default: null
    },
    labelClass: {
      type: String,
      default: null
    },
    selectClass: {
      type: String,
      default: null
    },
    name: {
      type: String,
      default: null
    },
    arrayItems: {
      type: Array,
      default: () => []
    },
    id: {
      type: String,
      default: null
    },
    label: {
      type: String,
      default: 'Босс'
    },
    required: {
      type: Boolean,
      default: false
    }
  },
  data: () => ({
    current: null
  })
}
</script>

<style scoped>
.form-select {
  font-family: 'Roboto', Arial, sans-serif;
  border: unset;
  margin: 0;
  padding: 0;
}

.form-select__label {
  font-size: .8rem;
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  gap: 0 0.5rem;
  cursor: pointer;
}

.form-select__item {
  font-size: .75rem;
  padding: .25rem .1rem;
  width: 100%;
  border-top: 0;
  border-right: 0;
  border-left: 0;
  border-bottom: 1px solid black;
  cursor: pointer;
}

.form-select__item:focus-within,
.form-select__item:focus {
  outline: 1px solid;
  border-radius: 4px;
}

.form-select__text {
  flex-shrink: 0;
  flex-basis: 4.5rem;
  white-space: nowrap;
}
</style>
