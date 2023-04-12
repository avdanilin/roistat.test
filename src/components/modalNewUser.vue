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

        <div class="modal__close" @click="toggleModal">&times;</div>
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
      return this.recursiveArraySelect(this.users).concat(this.defaultSelectValue).reverse()
    }
  },
  data: () => ({
    showModal: false,
    userName: '',
    userPhone: '',
    defaultSelectValue: [{name: 'без родителя', id: null}],
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
    recursiveArraySelect (users) {
      let result = []

      users.forEach(user => {
        result.push(user)

        if (user.children && user.children.length > 0) {
          result = result.concat(this.recursiveArraySelect(user.children))
        }
      })

      return result
    },
    handleSubmit () {
      try {
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
      } catch (error) {
        throw new Error(error)
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

.modal__close {
  position: absolute;
  font-size: 1rem;
  top: .25rem;
  right: .25rem;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 1rem;
  height: 1rem;
  border-radius: 50%;
  background-color: rgba(143, 133, 143, 0.2);
  cursor: pointer;
  transform: rotate(0);
  transition: transform .1s ease-in, background-color .3s ease-in-out;
}

.modal__close:hover {
  transform: rotate(-90deg);
  background-color: rgba(143, 133, 143, 0.5);
}

.modal__title {
  font-size: 1.2rem;
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
</style>
