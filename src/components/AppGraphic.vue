<template>
  <div :class="$style.invisible">
    {{plot()}}
  </div>
</template>

<script>
  import Plotly from 'plotly.js/dist/plotly-basic'

  export default {
    props: {
      gid: {
        required: true,
        type: String
      },
      days: {
        required: true,
        type: Array
      },
      monthLabel: {
        required: true,
        type: String
      },
      values: {
        required: true,
        type: Object
      }
    },
    methods: {
      plot () {
        let trace1 = {
          x: this.days,
          y: this.values.links,
          name: 'Переходы',
          type: 'scatter'
        }

        let trace2 = {
          x: this.days,
          y: this.values.money,
          name: 'Доход',
          yaxis: 'y2',
          type: 'scatter'
        }

        let data = [trace1, trace2]

        let layout = {
          title: this.monthLabel,
          width: 800,
          showlegend: false,
          xaxis: {domain: [1, this.days.length]},
          yaxis: {
            title: 'Переходы',
            titlefont: {color: '#1f77b4'},
            tickfont: {color: '#1f77b4'}
          },
          yaxis2: {
            title: 'Доход',
            titlefont: {color: '#ff7f0e'},
            tickfont: {color: '#ff7f0e'},
            anchor: 'free',
            overlaying: 'y',
            side: 'right',
            position: 1
          }
        }

        Plotly.newPlot(this.gid, data, layout)
      }
    }
  }
</script>

<style module>
  .invisible {
    display: none;
    position: absolute;
  }
</style>
