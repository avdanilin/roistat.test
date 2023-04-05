<template>
  <div>
    <button @click.prevent="toggleModal">Добавить</button>

    <transition name="fade-in-up">
      <div v-show="showModal" :class="['modal', { 'modal_show' : showModal }]">
        <h6 class="modal__title">{{ title }}</h6>

        <div class="modal__body">
          <div v-if="labelFirstName" class="modal__row">
            <formInput
              label-class="modal__label"
              input-class="modal__input"
              id="userName"
              type="text"
              :label="labelFirstName"
              :required="true"
            />
          </div>

          <div class="modal__row">
            <formInput
              label-class="modal__label"
              input-class="modal__input"
              id="userPhone"
              type="number"
              pattern="[0-9]+"
              :label="labelSecondName"
              :required="true"
            />
          </div>

          <div v-if="arrayItems" class="modal__row">
            <label class="modal__label" for="labelThirdName">{{ labelThirdName }}
              <!--            TODO component select-->
              <select name="labelThirdName" :id="labelThirdName" class="modal__input">
                <option v-for="(option, ix) in arrayItems" :key="ix" value="option.name">{{ option.name }}</option>
              </select>
            </label>
          </div>

          <div class="modal__row">
            <button class="modal__button" @click.prevent="addUser">Сохранить</button>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import formInput from './formInput.vue'
export default {
  name: 'modal',
  components: {formInput},
  props: {
    title: {
      type: String,
      default: 'Добавление пользователя'
    },
    labelFirstName: {
      type: String,
      default: 'labelFirstName'
    },
    labelSecondName: {
      type: String,
      default: 'labelSecondName'
    },
    labelThirdName: {
      type: String,
      default: 'labelThirdName'
    },
    arrayItems: {
      type: Array,
      default: null
    }
  },
  data: () => ({
    showModal: false,
    userName: '',
    userPhone: '',
    thirdName: ''
  }),
  methods: {
    toggleModal () {
      this.showModal = !this.showModal
    },
    addUser () {
      this.$emit('addUser')
    }
  }
}
</script>

<style scoped>
.modal {
  font-family: 'Roboto', Arial, sans-serif;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  width: fit-content;
  height: auto;
  padding: 1.2rem 1.75rem;
  box-shadow: 0 0 6px 1px #959494b0;
  border-radius: 4px;
}

.modal__title {
  font-size: 1rem;
  font-weight: 500;
  text-align: center;
  margin: 0;
}

.modal__body {
  margin-top: 1rem;
}

.modal__row:not(:first-child) {
    margin-top: 0.75rem;
}

.modal__row:last-child {
  text-align: center;
  margin-top: 1.25rem;
}

.modal__button {
  font-family: 'Roboto', Arial, sans-serif;
  font-size: .75rem;
  color: black;
  padding: .35rem .75rem ;
  border: 1px solid #2589FF;
  border-radius: 8px;
  cursor: pointer;
  background-color: unset;
  transition-property: background-color, color;
  transition-duration: .3s;
  transition-timing-function: ease-in-out;
}

.modal__button:hover {
  color: white;
  background-color: #2589FF;
}
</style>
