<template>
  <div id="app">
    <modalNewUser
      label-first-name="Имя"
      label-second-name="Телефон"
      label-third-name="Начальник"
      @addUser="addUser"
    />
  </div>
</template>

<script>
import modalNewUser from './components/modalNewUser.vue'

export default {
  name: 'listEmployees',
  components: {
    modalNewUser
  },
  methods: {
    addUser () {
      console.log('add')
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
}

::selection {
  background-color: #2589FF;
  color: white;
}

.fade-in-up-enter-active,
.fade-in-up-leave-active {
  transition: opacity .3s ease-in-out;
}
.fade-in-up-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
