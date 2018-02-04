<template>
  <div :id="gid"></div>
</template>

<script>
  import Plotly from 'plotly.js/dist/plotly-basic'

  export default {
    props: {
      gid: {
        required: true,
        type: String
      },
      labels: {
        required: true,
        type: Array
      },
      series: {
        required: true,
        type: Array
      }
    },
    mounted () {
      let trace1 = {
        x: [1, 2, 3],
        y: [4, 5, 6],
        name: 'yaxis1 data',
        type: 'scatter'
      }

      let trace2 = {
        x: [2, 3, 4],
        y: [40, 50, 60],
        name: 'yaxis2 data',
        yaxis: 'y2',
        type: 'scatter'
      }

      let data = [trace1, trace2]

      let layout = {
        title: 'multiple y-axes example',
        width: 800,
        xaxis: {domain: [0.3, 0.7]},
        yaxis: {
          title: 'yaxis title',
          titlefont: {color: '#1f77b4'},
          tickfont: {color: '#1f77b4'}
        },
        yaxis2: {
          title: 'yaxis2 title',
          titlefont: {color: '#ff7f0e'},
          tickfont: {color: '#ff7f0e'},
          anchor: 'free',
          overlaying: 'y',
          side: 'left',
          position: 0.15
        },
        yaxis3: {
          title: 'yaxis4 title',
          titlefont: {color: '#d62728'},
          tickfont: {color: '#d62728'},
          anchor: 'x',
          overlaying: 'y',
          side: 'right'
        },
        yaxis4: {
          title: 'yaxis5 title',
          titlefont: {color: '#9467bd'},
          tickfont: {color: '#9467bd'},
          anchor: 'free',
          overlaying: 'y',
          side: 'right',
          position: 0.85
        }
      }

      Plotly.newPlot(this.gid, data, layout)
    }
  }
</script>

<style scoped>

</style>
