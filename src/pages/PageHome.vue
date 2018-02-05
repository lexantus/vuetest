<template>
  <div>
    <h1 :class="$style.header">Статистика</h1>
    <UserDateShow @getData="getData"/>
    <AggregationGraphShow :data="d"/>
  </div>
</template>

<script>
  import UserDateShow from '@/components/UserDateShow'
  import AggregationGraphShow from '@/components/AggregationGraphShow'

  export default {
    components: {UserDateShow, AggregationGraphShow},
    methods: {
      getData (eventData) {
        this.d = eventData
      }
    },
    data () {
      return {
        d: {result: [], date: '2017-07-07'}
      }
    }
  }
</script>

<style module>
  .header {
    min-width: 768px;
  }
</style>
