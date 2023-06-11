<template>
  <div id="app">
    <Calculadora_Imc />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Calculadora_Imc from './components/Calculadora_Imc.vue';
export default defineComponent({
  name: 'App',
  components: {
    Calculadora_Imc,
  },
});
</script>

<style src="./styles/reset.css"></style>

<style src="./styles/style.css"></style>