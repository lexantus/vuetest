<template>
  <div>
    <form id="user_date_form" :class="$style.form">
      <label for="user_id">User ID: </label>
      <input :class="$style.input" id="user_id" type="text" v-model="userId"/>
      <label for="date_picker">Date: </label>
      <datepicker :class="$style.date" id="date_picker" v-model="date"/>
      <button :class="$style.button" type="submit" @click.prevent="handleClick">Submit</button>
    </form>
    <loading
      :show="isAjax"
      :label="label">
    </loading>
  </div>
</template>

<script>
  import datepicker from 'vue-datepicker2'
  import axios from 'axios'
  import loading from 'vue-full-loading'

  export default {
    components: {datepicker, loading},
    data () {
      return {
        userId: '24',
        date: (new Date()).toISOString().split('T')[0],
        isAjax: false,
        label: 'Loading...'
      }
    },
    methods: {
      handleClick () {
        this.isAjax = true
        if (this.date && this.userId) {
          axios.get(`/api/${this.date}/${this.userId}`).then(result => {
            console.log(result)
          }).catch(error => {
            this.isAjax = false
            console.log(error)
          })
        }
      }
    }
  }
</script>

<style module>
  .form {
    padding: 20px;
    border: 1px solid white;
    border-radius: 4px;
    margin-bottom: 20px;
    min-width: 768px;
  }

  .input {
    display: inline-block;
    padding: 6px;
    line-height: 22px;
    font-size: 16px;
    border: 2px solid rgb(255, 255, 255);
    box-shadow: rgba(0, 0, 0, 0.2) 0px 1px 3px 0px;
    border-radius: 2px;
    color: rgb(95, 95, 95);
    margin-right: 20px;
  }

  .date {
    padding: 10px;
    border-radius: 4px;
    margin-right: 20px;
  }

  .button {
    color: white;
    background-color: #212934;
    border-radius: 4px;
    padding: 9px 30px;
    line-height: 22px;
    font-size: 16px;
    cursor: pointer;
  }

  .button:hover {
    background-color: #0E2935;
  }
</style>
