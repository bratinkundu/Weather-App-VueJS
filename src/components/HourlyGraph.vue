<script>
import { Line } from "vue-chartjs";
export default {
  name: "Graph",
  extends: Line,
  props: {
    data: Array,
    label: Array,
  },
  data: function() {
    return {
      chartdata: {
        labels: this.label,
        datasets: [
          {
            label: null,
            backgroundColor: "#17CAFF",
            borderDashOffset: 2.0,
            data: this.data,
          },
        ],
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        legend: {
          display: false,
        },
        tooltips: {
          callbacks: {
            label: (tooltipItem) => `${tooltipItem.yLabel}°c`,
            title: () => null,
          },
        },
        scales: {
          yAxes: [
            {
              ticks: {
                // beginAtZero: true,
                stepSize: 0.5,
                suggestedMax: this.data[0] + 2,
                suggestedMin: this.data[0] - 2,
              },
              gridLines: {
                display: false,
                // color: "#FFFFFF",
              },
            },
          ],
          xAxes: [
            {
              ticks: {},
              gridLines: {
                display: false,
              },
            },
          ],
        },
      },
    };
  },
  mounted() {
    this.renderChart(this.chartdata, this.options);
  },
};
</script>

<style scoped>
canvas {
  height: 20vh;
  width: auto;
}
</style>
