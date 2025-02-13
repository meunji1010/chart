<template>
  <div>
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script setup>
  import { Chart, registerables } from 'chart.js';
  import { onMounted, ref } from 'vue';
  const chartCanvas = ref(null);

  Chart.register(...registerables);

  onMounted(()=>{
    new Chart(chartCanvas.value,{
      type: "bar",
      data: {
        labels: ['출석','지각','결석','조퇴'],
        datasets:[          
          {
            label: 'title',
            data: [10,5,10,2],
            backgroundColor:[
              'rgba(231,23,123,0.5)',
              'rgba(54,162,235,0.5)',
              'rgba(75,185,143,0.5)',
              'rgba(255,64,76,0.5)'
            ],
            borderColor:[
              'rgb(0,0,0)',
              'rgb(0,0,0)',
              'rgb(0,0,0)',
              'rgb(0,0,0)'
            ],
            borderWidth: 1
          }
        ]
      },
      options : {
        responsive: true,
        plugins: {
          title: {
            display: true,
            text: "Chart.js View"
          }
        }
      }
    });
  });
</script>

<style lang="scss" scoped>

</style>