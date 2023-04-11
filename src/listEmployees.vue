<template>
  <div id="app">
    <tableEmployees
      v-if="users.length > 0"
      :users="users"
    />

    <modalNewUser
      label-first-name="Имя"
      label-phone="Телефон"
      label-boss="Начальник"
      :users="users"
      @handleSubmit="addUser"
    />
  </div>
</template>

<script>
import modalNewUser from './components/modalNewUser.vue'
import tableEmployees from './components/tableEmployees.vue'

export default {
  name: 'listEmployees',
  components: {
    modalNewUser,
    tableEmployees
  },
  data: () => ({
    name: '',
    parent: '',
    users: JSON.parse(localStorage.getItem('users')) || [],
    id: null
  }),
  created () {
    this.id = 0
  },
  methods: {
    addUser ({name, phone, bossID = null}) {
      const user = {
        id: this.id++,
        name,
        phone,
        children: [],
        parentID: bossID
      }

      if (bossID !== null) {
        const parent = this.findUserById(bossID)

        if (parent) {
          parent.children.push(user)
        } else {
          this.users.push(user)
        }
      } else {
        this.users.push(user)
      }

      localStorage.setItem('users', JSON.stringify(this.users))
    },
    findUserById (id, users = this.users) {
      for (const user of users) {
        if (user.id === id) {
          return user
        }

        if (user.children.length > 0) {
          const nestedUser = this.findUserById(id++, user.children)

          if (nestedUser) {
            return nestedUser
          }
        }
      }

      return null
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;900&display=swap');

html,
body {
  font-family: 'Roboto', Arial, sans-serif;
  letter-spacing: 0.01rem;
  word-break: break-word;
  font-size: 18px;
  margin: 0;
}

::selection {
  background-color: #2589FF;
  color: white;
}

html,
body,
#app {
  height: 100%;
}

.fade-in-enter-active,
.fade-in-leave-active {
  transition: all .2s linear;
}

.fade-in-enter,
.fade-leave-to {
  opacity: 0;
}

.backdrop {
  position: fixed;
  inset: 0;
  background-color: rgba(112, 112, 112, 0.5);
  z-index: 0;
}

.place-center {
  display: flex;
  justify-content: center;
}

.button {
  font-family: 'Roboto', Arial, sans-serif;
  font-size: .75rem;
  color: black;
  padding: .35rem .75rem;
  border: 1px solid #2589FF;
  border-radius: 8px;
  cursor: pointer;
  background-color: unset;
  transition-property: background-color, color;
  transition-duration: .3s;
  transition-timing-function: ease-in-out;
}

.button:hover {
  color: white;
  background-color: #2589FF;
}

.button_outer {
  margin: 0.5rem;
}
</style>
