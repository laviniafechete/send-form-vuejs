<template>
  <div id="app">
      <div class="content">
          <img src="./assets/logo.png" alt="Logo" id="logo">
          <formsend></formsend>
          <p class="footer">THIS IS JUS A FRONT-END TEST <br> FOR GREENHORSEGAMES</p>
      </div>
  </div>
</template>

<script>
import './assets/style.css'
import formsend from './components/FormSend.vue'

export default {
  name: 'app',
  components: {
    formsend
  }
};
</script>
