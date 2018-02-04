<template>
  <div :class="$style.container">
    <div :class="$style.aggregation">
      <div :class="$style.row">
        <span>СРА </span><span>404руб</span>
      </div>
      <div :class="$style.row">
        <span>Доход </span><span>409898руб</span>
      </div>
    </div>
    <AppGraphic :class="$style.graph" :gid="'result'" :labels="labels" :series="series"/>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri'],
        labels1: ['Alex', 'Katya', 'Yaroslav'],
        series: [
          [5, 2, 4, 2, 0]
        ]
      }
    }
  }
</script>

<style module>
  .container {
    padding: 10px;
    border: 1px solid white;
    border-radius: 4px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    overflow: scroll;
    height: 400px;
    min-width: 768px;
  }

  .aggregation {
    background-color: white;
    border-radius: 4px;
    min-width: 200px;
    margin-right: 20px;
  }

  .row {
    display: flex;
    justify-content: space-between;
    color: white;
    background-color: #212934;
    border-bottom-left-radius: 4px;
    border-bottom-right-radius: 4px;
    padding: 10px;
  }

  .row:first-child {
    border-top-left-radius: 4px;
    border-top-right-radius: 4px;
  }

  .row:nth-child(odd) {
    background-color: white;
    color: #212934;
  }

  .graph {
    height: 100%;
  }
</style>
