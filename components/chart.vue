
<script>
import { Line, mixins } from 'vue-chartjs'
export default {
  mixins: [Line],
  props: ['data', 'options'],
  name: "chart",
  data() {
    return {
      barChartData: {
        labels: [
          '2019-06',
          '2019-07',
          '2019-08',
          '2019-09',
          '2019-10',
          '2019-11',
          '2019-12',
          '2020-01',
          '2020-02',
          '2020-03',
          '2020-04',
          '2020-05'
        ],
        datasets: [
          {
            label: 'Visits',
            display: false,
            data: [10, 15, 20, 30, 40, 50, 60, 70, 34, 45, 11, 78, 45],
            // backgroundColor: '',
            borderColor:'#22543d',
            pointBackgroundColor: '#22543d'
          },
        ]
      },
      barChartOptions: {
        responsive: true,
        legend: {
          display: false
        },
        title: {
          display: false,
          text: 'ds',
          fontSize: 24,
          fontColor: '#6b7280'
        },
        scales: {
          xAxes: [
            {
              ticks: {
                display:false
              },
              gridLines: {
                display: false
              }
            }
          ],
          yAxes: [
            {
              ticks: {
                beginAtZero: false,
                display:false,

              },
              gridLines: {
                display: false
              }
            }
          ]
        }
      }
    }
  },
  mounted () {
    this.barChartData.datasets[0].data = this.data.price
    this.barChartData.labels = this.data.createdAt
    this.renderChart(this.barChartData, this.barChartOptions)
  }
}
</script>

<style scoped>

</style>
