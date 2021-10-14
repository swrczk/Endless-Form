<template>
  <div >
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <CustomForm /> -->
    <QuestionsForm />
  </div>
</template>

<script>
// import CustomForm from './components/CustomForm.vue'
import QuestionsForm from './components/QuestionsForm.vue'

export default {
  name: 'App',
  components: {
    // CustomForm,
    QuestionsForm
  }
}
import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap-vue/dist/bootstrap-vue.css";
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
