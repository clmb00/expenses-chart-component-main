<script>

import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
} from 'chart.js'

import { Bar } from 'vue-chartjs'

ChartJS.register(CategoryScale, LinearScale, BarElement, Title, Tooltip, Legend)

Tooltip.positioners.myCustomPositioner = function(elements, eventPosition) {
    // A reference to the tooltip model
    const tooltip = this;

    const pos = Tooltip.positioners.average(elements);

    return {
        x: pos.x,
        y: pos.y - 26
    };
};

export default{
  name: 'ExpensesChart',
  components:{
    Bar
  },
  props:{
    myData: Array
  },
  data(){
    return{
      thisMonth: this.myData[1].amount,
      lastMonth: this.myData[2].amount,
      total: this.myData[3].amount,
      data: {
        labels: ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun'],
        datasets: [{ 
          data: this.getDataChart(),
          backgroundColor: this.getArrayColors('rgba(118, 181, 188)', 'rgb(236, 119, 95)'),
          hoverBackgroundColor: this.getArrayColors('rgb(180, 223, 229)', 'rgb(255, 155, 135)'),
          borderRadius: 5,
          borderSkipped: false
        }]
      },
      options:{
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: {
            display: false
          },
          tooltip: {
            position: 'myCustomPositioner',
            xAlign: 'center',
            backgroundColor: 'rgb(56, 35, 20)',
            displayColors: false,
            padding: {
              x: 8,
              y: 10
            },
            caretSize: 0,
            bodyFont: {
              family: "'DM Sans', sans-serif",
              size: 16,
              weight: 700
            },
            callbacks: {
              label: function(context){
                return '$' + context.raw;
              },
              title: function(){
                return ''
              }
            }
          }
        },
        scales: {
          y: {
            display: false,
            max: 74
          },
          x: {
            ticks: {
              color: 'rgb(147, 134, 123)',
              font: {
                family: "'DM Sans', sans-serif",
                size: 14
              }
            },
            grid: {
              display: false
            },
            border: {
              display: false
            }
          }
        }
      }
    }
  },
  computed:{
    calcuatePercentage(){
      let percentage = ((this.thisMonth - this.lastMonth) * 100 / this.lastMonth).toFixed(1);
      if(percentage >= 0){
        return '+' + percentage + '%';
      } else {
        return percentage + '%';
      }
    }
  },
  methods:{
    getDataChart(){

      let dataChart = [];
      
      this.myData[0].data.forEach(element => {
        dataChart.push(element.amount);
      });
      return dataChart
    },
    getArrayColors(generalColor, activeColor){
      
      let colorChart = [];
      let date = new Date().getDay();

      for (let i = 0; i < this.myData[0].data.length; i++) {
        i == date ? colorChart.push(generalColor) : colorChart.push(activeColor);
      }
      return colorChart
    }
  }
}

</script>

<template>

  <div class="card">

    <div class="top_card">
      <div class="info">
        <p>My balance</p>
        <p>${{total}}</p>
      </div>
      <div class="logo">
        <img src="../assets/images/logo.svg" alt="logo">
      </div>
    </div>
  
    <div class="bottom_card">
      <h3>Spending - Last 7 days</h3>
      <div class="chart">
        <Bar :data="data" :options="options" />
      </div>
      <div class="bottom_infos">
        <div class="left">
          <p class="text_info">Total this month</p>
          <p>${{thisMonth}}</p>
        </div>
        <div class="right">
          <p>{{calcuatePercentage}}</p>
          <p class="text_info">from last month</p>
        </div>
      </div>
    </div>

  </div>
  
</template>

<style lang="scss" scoped>

@use '../styles/variables' as *;

.card{
  width: 80%;
  max-width: 540px;
  flex-shrink: 1;
  flex-grow: 1;
  margin: 1em;

  & > *{
    border-radius: 1.5rem;
    padding: 30px;
  }
}

.top_card{
  background-color: $primary_hot;
  display: flex;
  justify-content: space-between;
  margin-bottom: 25px;

  p:first-child{
    color: $neutral_l;
  }
  p:last-child{
    color: $neutral_ll;
    font-size: 2rem;
    font-weight: bold;
    margin-top: .5rem;
  }

  .logo{
    display: flex;
    align-items: center;
  }
}

.bottom_card{
  background-color: $neutral_ll;
  width: 100%;

  h3{
    color: $neutral_dd;
    font-size: 2rem;
  }

  .bottom_infos{
    display: flex;
    justify-content: space-between;
    padding-top: 30px;

    .text_info{
      color: $neutral_d;
    }

    .left p:last-child{
      font-size: 3rem;
      color: $neutral_dd;
      font-weight: bold;
    }

    .right{
      text-align: right;
      margin-top: auto;
      margin-bottom: 10px;

      & p:first-child{
        color: $neutral_dd;
        font-weight: bold;
      }
    }
  }
}

.chart{

  position: relative;
  width: 100%;
  height: 268px;

  & *:first-child{
    margin-bottom: 30px;
  }
  &::after{
    content: '';
    display: block;
    width: 100%;
    height: 2px;
    background-color: $neutral_l;
    margin-bottom: 30px;
  }
}

// 440
@media screen and (max-width: 440px) {

  .card{
    & > *{
      padding: 20px;
    }
  }

  .top_card{
    p:last-child{
      font-size: 1.8rem;
    }
  }

  .bottom_card{
    
    h3{
      font-size: 1.4rem;
    }

    .bottom_infos{
      .left p:last-child{
        font-size: 1.5rem;
      }

      .right{
        margin-bottom: 2px;
      }
    }

  }
  
}

</style>
