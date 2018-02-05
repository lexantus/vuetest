<template>
  <div>
    <div :class="$style.errors" v-if="errors.length">
      <b>Исправьте следующие ошибки:</b>
      <ul>
        <li v-for="error in errors" :key="error">{{ error }}</li>
      </ul>
    </div>
    <form id="user_date_form" :class="$style.form">
      <label for="user_id">User ID: </label>
      <input :class="$style.input" id="user_id" type="text" v-model="userId" @input="checkErrors"/>
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
  import moment from 'moment'

  export default {
    components: {datepicker, loading},
    data () {
      return {
        errors: [],
        userId: '24',
        date: moment().format('YYYY-MM-DD'),
        isAjax: false,
        label: 'Loading...'
      }
    },
    methods: {
      checkErrors () {
        this.errors = []
        if (!this.date) this.errors.push('Не выбрана Data')
        if (!this.userId) this.errors.push('Не указан User ID')
      },
      handleClick () {
        if (this.date && this.userId) {
          this.isAjax = true
          axios.get(`/api/${this.date}/${this.userId}`).then(r => {
            this.isAjax = false

            let data = r.data

            if (data.status === 'ok') {
              this.$emit('getData', {
                result: data.result,
                date: this.date
              })
            }
          }).catch(error => {
            this.isAjax = false
            console.log(error)
            this.errors = ['Ошибка сервера']
          })
        } else {
          this.checkErrors()
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

  .errors {
    color: darkred;
    border: 1px solid darkred;
    padding: 10px;
    margin-bottom: 20px;
    border-radius: 4px;
  }
</style>
