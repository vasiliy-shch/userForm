<template>
  <div id="app">
    <user-form/>
  </div>
</template>

<script>
import UserForm from '@/components/UserForm.vue'
export default {
  components: {
    UserForm,
  }
}
</script>

<style>
@import url('../src/assets/css/fonts.css');
html, body {
    margin: 0;
    font-family: 'Montserrat', sans-serif;
    scroll-behavior: smooth;
}
#app {
    font-family: 'Montserrat', sans-serif;
}
</style>
