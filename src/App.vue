<template>
  <div class="app">
    <h1>Hat Loss Probability Calculator</h1>
    <HatForm />
  </div>
</template>

<script>
import HatForm from './components/HatForm.vue';

export default {
  name: 'App',
  components: {
    HatForm,
  },
};
</script>

<style>
@import './assets/style.min.css';
</style>
