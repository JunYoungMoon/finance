<template>
  <div class="chart">
    <canvas id="ExchageRatesChart" width="1000" height="700"></canvas>
    <button v-on:click="refresh">resetZoom</button>
  </div>
</template>

<script>
import { Chart, registerables } from "chart.js";
import zommPlugin from "chartjs-plugin-zoom";
Chart.register(...registerables, zommPlugin);
import jsonData from "../../../tempData/exchagerates.json";
import Vue from "vue";

export default Vue.extend({
  data() {
    return {
      ctx: "",
      config: "",
      chart: Object,
    };
  },
  methods: {
    refresh() {
      this.chart.resetZoom();
    },
  },
  mounted() {
    this.ctx = document.getElementById("ExchageRatesChart");

    this.config = {
      type: "line",
      data: {
        labels: [],
        datasets: [
          {
            label: "환율 정보",
            data: [],
            pointRadius: 0,
            backgroundColor: [
              "rgba(255, 99, 132, 0.2)",
              "rgba(54, 162, 235, 0.2)",
              "rgba(255, 206, 86, 0.2)",
              "rgba(75, 192, 192, 0.2)",
              "rgba(153, 102, 255, 0.2)",
              "rgba(255, 159, 64, 0.2)",
            ],
            borderColor: [
              "rgba(255, 99, 132, 1)",
              "rgba(54, 162, 235, 1)",
              "rgba(255, 206, 86, 1)",
              "rgba(75, 192, 192, 1)",
              "rgba(153, 102, 255, 1)",
              "rgba(255, 159, 64, 1)",
            ],
            borderWidth: 1,
          },
        ],
      },
      options: {
        scales: {
          y: {
            beginAtZero: false,
          },
        },
        plugins: {
          zoom: {
            pan: {
              enabled: true,
            },
            zoom: {
              wheel: {
                enabled: true,
              },
            },
          },
        },
      },
    };

    jsonData.forEach((e) => {
      this.config.data.datasets[0].data.push(Number(e.KRW));
      this.config.data.labels.push(e.date);
    });

    this.chart = new Chart(this.ctx, this.config);
  },
});
</script>

<style scoped>
.chart {
  float: left;
  padding: 2rem;
  border: 1px solid red;
  width: 1000px;
}
</style>
