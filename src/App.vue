<script>

import ExpensesChart from './components/ExpensesChart.vue';

import json from './assets/data.json';

export default{
  name: 'App',
  components:{
    ExpensesChart
  },
  data(){
    return{
      myData: json
    }
  }
}

</script>

<template>

  <div class="container">
    <ExpensesChart :myData="myData"/>
  </div>
  
</template>

<style lang="scss">

@use './styles/general.scss';

.container{
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

</style>
