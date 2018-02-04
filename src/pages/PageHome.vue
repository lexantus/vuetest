<template>
  <div>
    <h1 :class="$style.header">Welcome to app</h1>
    <UserDateShow/>
    <AggregationGraphShow/>
  </div>
</template>

<script>
  import UserDateShow from '@/components/UserDateShow'
  import AggregationGraphShow from '@/components/AggregationGraphShow'

  export default {
    components: {UserDateShow, AggregationGraphShow}
  }
</script>

<style module>
  .header {
    min-width: 768px;
  }
</style>
