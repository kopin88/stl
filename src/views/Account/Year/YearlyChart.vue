<script>
import { Line } from 'vue-chartjs'
import { get, apiDomain } from '../../../helpers/api'
export default {
extends: Line,
props: ['year_id'],
  data () {
    return {
      incomes: [],
      expenses: [],
      balances: [],
      labels: [],
      options: {
        scales: {
          yAxes: [{
            ticks: {
              beginAtZero: true
            },
            gridLines: {
              display: true
            }
          }],
          xAxes: [ {
            gridLines: {
              display: false
            }
          }]
        },
        legend: {
          display: false
        },
        responsive: true,
        maintainAspectRatio: false
      }
    }
  },
  mounted () {
    get(apiDomain + `/years/${this.year_id}`)
      .then(resp => {
        // console.log('resp', resp.data.data)
        this.labels = resp.data.monthNames
        this.incomes = resp.data.inTotals
        this.expenses = resp.data.outTotals
        this.balances = resp.data.balances
        this.setUpGraph()
      })
  },
  methods: {
    setUpGraph() {
      this.renderChart({
        labels: this.labels,
        datasets: [
          {
            label: 'Inocmes',
            borderColor: '#249EBF',
            pointBackgroundColor: 'white',
            borderWidth: 1,
            pointBorderColor: '#249EBF',
            backgroundColor: 'transparent',
            data: this.incomes
          },
          {
            label: 'Expenses',
            borderColor: '#FF5722',
            pointBackgroundColor: 'white',
            borderWidth: 1,
            pointBorderColor: '#FF5722',
            backgroundColor: 'transparent',
            data: this.expenses
          },
          {
            label: 'Balance',
            borderColor: '#4CAF50',
            pointBackgroundColor: 'white',
            borderWidth: 1,
            pointBorderColor: '#4CAF50',
            backgroundColor: 'transparent',
            data: this.balances
          },
        ]
      }, this.options)
    }
  }
}
</script>
