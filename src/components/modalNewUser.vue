<template>
  <div>
    <div class="place-center">
      <button type="button" class="button button_outer" @click.prevent="toggleModal">Добавить</button>
    </div>
    <transition name="fade-in">
      <div v-if="showModal" @click="toggleModal" class="backdrop"></div>
    </transition>

    <transition name="fade-in">
      <div v-if="showModal" :class="['modal', { 'modal_show' : showModal }]">
        <!--TODO: times button-->
        <h6 class="modal__title">{{ title }}</h6>

        <form @submit.prevent="handleSubmit" class="modal__body">
            <formInput
              v-if="labelFirstName"
              ref="firstName"
              item-class="modal__row"
              label-class="modal__label"
              input-class="modal__input"
              id="userName"
              type="text"
              :label="labelFirstName"
              :required="true"
            />

            <formInput
              v-if="labelPhone"
              ref="inputPhone"
              item-class="modal__row"
              label-class="modal__label"
              input-class="modal__input"
              id="userPhone"
              type="number"
              pattern="[0-9]+"
              :label="labelPhone"
              :required="true"
            />

            <formSelect
              v-if="users.length > 0 && labelBoss"
              ref="selectBoss"
              item-class="modal__row"
              label-class="modal__label"
              select-class="modal__select"
              id="boss"
              :label="labelBoss"
              :array-items="computedArraySelect"
            />

          <div class="modal__row">
            <button type="submit" class="button">Сохранить</button>
          </div>
        </form>
      </div>
    </transition>
  </div>
</template>

<script>
import formInput from './formInput.vue'
import formSelect from './formSelect.vue'
export default {
  name: 'modal',
  components: {
    formSelect,
    formInput
  },
  props: {
    title: {
      type: String,
      default: 'Добавление пользователя'
    },
    labelFirstName: {
      type: String,
      default: null
    },
    labelPhone: {
      type: String,
      default: null
    },
    labelBoss: {
      type: String,
      default: null
    },
    users: {
      type: Array,
      default: null
    }
  },
  computed: {
    computedArraySelect () {
      return this.users.concat(this.users.find(user => user.children.flat()).children)
    }
  },
  data: () => ({
    showModal: false,
    userName: '',
    userPhone: '',
    boss: null
  }),
  methods: {
    toggleModal () {
      this.showModal = !this.showModal

      if (this.showModal) {
        this.focusFirstElement()
      }
    },
    focusFirstElement () {
      setTimeout(() => {
        const firstInput = this.$refs.firstName.$el.querySelector('.modal__input') || null

        if (firstInput) {
          firstInput.focus()
        }
      })
    },
    handleSubmit () {
      const refName = this.$refs.firstName
      const refPhone = this.$refs.inputPhone
      const refBoss = this.$refs.selectBoss

      const inputName = refName.$el.querySelector('.modal__input') || null
      const inputPhone = refPhone.$el.querySelector('.modal__input') || null
      const selectBoss = refBoss ? refBoss.$el.querySelector('.modal__select') : null

      if (inputName && inputPhone) {
        let nameValue = inputName.value
        let phoneValue = inputPhone.value

        const objectForm = {
          name: nameValue,
          phone: phoneValue
        }

        if (selectBoss) {
          Object.assign(objectForm, {
            bossID: parseInt(selectBoss.value)
          })
        }

        this.$emit('handleSubmit', objectForm)
        nameValue = ''
        phoneValue = ''
        if (selectBoss) {
          selectBoss.value = ''
        }
        this.showModal = false
      }
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

.modal__text {
  flex-shrink: 0;
}
</style>
