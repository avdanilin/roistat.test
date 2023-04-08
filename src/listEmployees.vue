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
      @handleSubmit="handleSubmit"
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
  mounted () {
    this.id = this.users.length || 0
  },
  methods: {
    handleSubmit (data) {
      const user = {
        id: this.id++,
        name: data.name,
        phone: data.phone,
        // boss: data.boss
      }

      this.users.push(user)
      localStorage.setItem('users', JSON.stringify(this.users))
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
</style>
