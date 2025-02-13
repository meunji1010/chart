<template>
  <div class="date-analysis">
    <h2>일자별 출결현황</h2>
    <input type="date" v-model="selectedDate">
    <div class="chart-container" v-if="isDate">
      <canvas ref="chartCanvas"></canvas>
    </div>
    <!-- 데이터가 없다면 -->
    <p v-else>해당 날짜의 출결 데이터가 없습니다.</p>
  </div>
</template>

<script setup>
import { Chart, registerables } from 'chart.js';
import { nextTick, ref, watch } from 'vue';

  const props = defineProps({
    students : Array
  });
  const selectedDate = ref('');
  const chartCanvas = ref('');
  let chartInst = null;
  const isDate = ref(null);
  Chart.register(...registerables);
  watch( selectedDate, async ()=>{
    // console.log(selectedDate.value); //2025-02-11
    const attendanceCount = { present:0, leave:0 , absent:0, late:0 };
    isDate.value = false;
    props.students.forEach((list)=>{
      list.attendance.forEach((item)=>{
        if(item.date === selectedDate.value ){
          attendanceCount[item.status]++;
          isDate.value = true;
        }
      });
    });
    //새로 만들기 위해, 기존 차트 삭제
    if( chartInst ) {
      chartInst.destroy();
    }
    //데이터가 없다면 차트를 생성할 필요X
    if( !isDate.value )return;
    //DOM 업데이트가 되면 재갱신
    await nextTick();
    //차트 그리기
    chartInst = new Chart(chartCanvas.value,{
      type: 'bar',
      data: {
        labels: ['출석','지각','결석','조퇴'],
        datasets:[{
          label: '출결현황',
          data: [attendanceCount.present, attendanceCount.late, attendanceCount.absent, attendanceCount.leave],
          backgroundColor: ['orange','red','green','blue']
        }]
      },
      options: {
        responsive: true,
        plugins: {
          legend: {position: 'bottom'}
        }
      }
    });
  } );
</script>

<style lang="scss" scoped>
.date-analysis > p{
  margin-top: 50%;
  }
  .chart-container{
    width: 100%;
    max-width: 300px;
    margin: auto;
  }
  input{
    width: 70%;
    height: 1.7rem;
    font-size: 1.5rem;
  }
</style>