<template>
  <div>
    <div v-if="data.result.length" :class="$style.container">
      <div :class="$style.aggregation">
        <div v-for="(value, key) in getSummary(data.result)" :class="$style.row" :key="key">
          <span>{{getLabel(key)}}</span><span>{{key === 'PAYMENT' ? moneyFormat(value) : value}}</span>
        </div>
      </div>
      <div :id="'graphic'" :class="$style.graph"></div>
      <AppGraphic :gid="'graphic'"
                  :days="getDays()"
                  :monthLabel="getMonthLabel()"
                  :values="getValues()"/>
    </div>
    <div v-if="data.result.length === 0" :class="$style.message">Нет данных</div>
  </div>
</template>

<script>
  import moment from 'moment'
  import accounting from 'accounting'

  export default {
    props: {
      data: {
        required: true,
        type: Object
      }
    },
    methods: {
      moneyFormat (money) {
        return accounting.formatMoney(1 * money, '₽', 0, '.', ',', '%v %s')
      },
      getLabel (type) {
        switch (type) {
          case 'LINK_VISITOR':
            return 'Переходы'
          case 'REGISTRATION':
            return 'Регистрации'
          case 'PAYMENT':
            return 'Доход'
        }
      },
      getSummary (data) {
        return data.reduce((memo, obj) => {
          if (obj.event === 'LINK_VISITOR' || obj.event === 'REGISTRATION' || obj.event === 'PAYMENT') {
            if (typeof memo[obj.event] === 'undefined') {
              memo[obj.event] = 0
            }
            if (obj.event === 'LINK_VISITOR' || obj.event === 'REGISTRATION') {
              memo[obj.event]++
            } else {
              memo[obj.event] += obj.event_value
            }
          }
          return memo
        }, {})
      },
      getMonthLabel () {
        return moment(this.data.date.split('-')[1], 'MM').format('MMMM')
      },
      getDaysNum () {
        return new Date(this.data.date.split('-')[0], this.data.date.split('-')[1], 0).getDate()
      },
      getDays () {
        const numDays = this.getDaysNum()
        let days = []
        for (let i = 0; i < numDays; i++) {
          days.push(i + 1)
        }
        return days
      },
      getValues () {
        const numDays = this.getDaysNum()
        let temp = []
        let tempMoney = []
        let links = []
        let money = []

        const d = this.data.result.map(
          value => {
            return {type_date: `${value.event},${moment(value.date, moment.ISO_8601).format('YYYY-MM-DD')}`, ...value}
          }
        ).reduce(
          (memo, obj) => {
            if (typeof memo[obj.type_date] === 'undefined') {
              memo[obj.type_date] = {}
              memo[obj.type_date].sum = 0
            }
            if (obj.event === 'REGISTRATION' || obj.event === 'LINK_VISITOR') {
              memo[obj.type_date].sum++
            } else {
              memo[obj.type_date].sum += obj.event_value
            }

            memo[obj.type_date].event = obj.event
            memo[obj.type_date].day = moment(obj.date, moment.ISO_8601).format('D')
            return memo
          }, {}
        )

        temp = Object.values(d).filter(
          value => {
            return value.event === 'LINK_VISITOR'
          }
        ).map(
          value => {
            return {sum: value.sum, day: value.day}
          }
        ).sort(function (a, b) {
          let t1 = +a.day
          let t2 = +b.day
          if (t1 < t2) return -1
          if (t1 > t2) return 1
          return 0
        })

        tempMoney = Object.values(d).filter(
          value => {
            return value.event === 'PAYMENT'
          }
        ).map(
          value => {
            return {sum: value.sum, day: value.day}
          }
        ).sort(function (a, b) {
          let t1 = +a.day
          let t2 = +b.day
          if (t1 < t2) return -1
          if (t1 > t2) return 1
          return 0
        })

        let b1 = true
        let b2 = true
        let el1
        let el2
        for (let i = 0; i < numDays; i++) {
          if (b1) {
            el1 = temp.shift()
          }
          if (el1 && el1.day === '' + (i + 1)) {
            links.push(el1.sum)
            b1 = true
          } else {
            links.push(0)
            b1 = false
          }

          if (b2) {
            el2 = tempMoney.shift()
          }
          if (el2 && el2.day === '' + (i + 1)) {
            money.push(el2.sum)
            b2 = true
          } else {
            money.push(0)
            b2 = false
          }
        }
        return {links, money}
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

  .message {
    border: 1px solid #212934;
    width: 100%;
    line-height: 50px;
    border-radius: 4px;
    text-align: center;
  }
</style>
