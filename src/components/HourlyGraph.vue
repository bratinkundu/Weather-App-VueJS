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
            backgroundColor: "#f87979",
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
      },
    };
  },
  mounted() {
    this.renderChart(this.chartdata, this.options);
  },
};
</script>
