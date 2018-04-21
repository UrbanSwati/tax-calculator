<template>
  <div id="app">
   <div class="container">
     <h1 class="text-center">Tax Calculator</h1>
     <app-learn-more></app-learn-more>
    <app-calculator></app-calculator>
   </div>
  </div>
</template>

<script>
import Calculator from './components/Calculator.vue';
import LearnMore from './components/LearnMore.vue';
export default {
  components: {
    appCalculator : Calculator,
    appLearnMore : LearnMore
  }
}
</script>

<style>

</style>
