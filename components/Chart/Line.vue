<script>
import { Line } from 'vue-chartjs'

export default {
  extends: Line,
  props: {
    chartoptions: {
      default: () => ({}),
    },
    chartdata: {
      default: () => ({
        labels: ['a1', 'a2', 'a3'],
        datasets: [
          {
            fill: false,

            label: 'Максимум',
            data: [91, 85, 97],
            borderWidth: 2,
            backgroundColor: false,
            borderColor: 'lime',
          },

          {
            fill: false,

            label: 'Средний',
            data: [75, 65, 77],
            borderWidth: 2,
            backgroundColor: false,
            borderColor: 'green',
          },
          {
            fill: false,

            label: 'Минимум',
            data: [61, 45, 57],
            borderWidth: 2,
            backgroundColor: false,
            borderColor: 'red',
          },

          {
            fill: false,

            label: 'Рейтинг в классификации',
            data: [72, 80, 60],
            borderWidth: 2,
            backgroundColor: false,
            borderColor: 'blue',
          },
        ],
      }),
    },
  },
  mounted() {
    if (this.chartdata != false)
      this.renderChart(this.chartdata, this.chartoptions)
  },
  watch: {
    chartdata: {
      handler(newValue) {
        if (newValue != false) {
          this.renderChart(this.chartdata, this.chartoptions)
        }
      },
      deep: true,
    },
  },
}
</script>
