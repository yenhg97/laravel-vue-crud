<template>
  <div>
    <DataChart :chartdata="chart1Data" :options="chartOptions" />
    <DataChart v-if="loaded" :chartdata="chart2Data" :options="chartOptions" />
  </div>
</template>

<script>
import DataChart from "./DataChart.vue";
import json from "./json/test.json";

export default {
  components: {
    DataChart,
  },
  data() {
    return {
      loaded: false,
      chart1Data: {
        labels: json.labels,
        datasets: [
          {
            label: "My First dataset",
            data: json.data,
          },
        ],
      },
      chart2Data: {
        labels: json.labels,
        datasets: [
          {
              label: "My First dataset",
              data: [],
          },
        ],
      },
      chartOptions: {
        scales: {
          xAxes: [
            {
              ticks: {
                min: 0,
                max: 100,
                stepSize: 50,
              },
            },
          ],
        },
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },
  async mounted () {
    this.loaded = false;
    try {
      const response = await this.axios.get("http://localhost:8000/api/data/");
      this.chart2Data.datasets[0].data = response.data.map((a) => a.value);
      this.loaded = true
    } catch (e) {
      console.error(e)
    }
  },
};
</script>
