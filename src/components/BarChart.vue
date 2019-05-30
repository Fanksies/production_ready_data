<template>
  <div id="barChart" class="card p-3 shadow-sm">
    <canvas v-bind:id="canvasID" width="400" height="400"></canvas>
  </div>
</template>

<script>
import Chart from 'chart.js';

export default {
  name: "BarChart",
  props: {
    canvasID: String,
    labels: Array,
    data: Array
  },
  methods: {
    drawBarChart(id, labels, data) {
      var ctx = document.getElementById(id);
      //console.log(ctx);

      var myChart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: labels,
          datasets: [{
            label: '# of Apps',
            data: data,
            backgroundColor: 'rgba(255, 99, 132, 0.6)',
            borderColor: 'rgba(255, 99, 132, 1)',
            borderWidth: 1
          }]
        },
        options: {
          scales: {
            yAxes: [{
              ticks: {
                beginAtZero: true
              }
            }]
          }
        }
      });
    }
  },
  mounted() {
    //console.log(this.canvasID, this.labels, this.data);
    this.drawBarChart(this.canvasID, this.labels, this.data);
  }
}
</script>

<style lang="scss">

</style>
