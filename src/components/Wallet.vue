<template>
  <div id="wallet">
    <!--  Transfer panel -->
    <Transfer
      :showTransferPanel="showTransferPanel"
      :toggleTransferPanel="toggleTransferPanel"
      :onCompleted="addTransfer"
      :account="account"
      :contacts="contacts"
    />

    <!--  The wallet -->
    <div class="wallet__content">
      <h1>Wallet</h1>
      <button
        type="button"
        name="button"
        class="btn btn--big"
        :disabled="account.balance <= 0"
        v-on:click="toggleTransferPanel()">
        New transfer
      </button>
      <div class="wallet__alias">
        <span>
          <span>{{ account.alias }}</span>
          <span>Alias</span>
        </span>
      </div>
      <div class="table__block wallet__account">
        <span class="account__address">
          <span>{{ account.address }}</span>
          <span>Address</span>
        </span>
        <span class="account__balance">
          <span>{{ formatAmount(account.balance) }}</span>
          <span>Balance</span>
        </span>
        <span>
          <span>{{ account.coin.toUpperCase() }}</span>
          <span>Token</span>
        </span>
      </div>
      <ul class="wallet__movements">
        <h3>Last movements</h3>
        <li class="table__block">
          <span class="movements__time movements__time--title">Date</span>
          <span class="movements__type"></span>
          <span class="movements__user">Recipient</span>
          <span class="movements__amount">Amount</span>
          <span class="movements__balance">Balance</span>
        </li>
        <li class="table__block" v-bind="account.movements" v-for="movement in account.movements">
          <span class="movements__time">
            <span>{{ getTime(movement.timestamp) }}</span>
            <span>{{ getDate(movement.timestamp) }}</span>
          </span>
          <span v-bind:class="[movement.type === 'in' ? 'green_dark' : 'red_warning', 'movements__type']">
            {{ movement.type}}
          </span>
          <span class="movements__user">
            <span class="movements__address">
              {{ movement.address}}
            </span>
            <span class="movements__alias">
              {{ movement.alias}}
            </span>
          </span>
          <span v-bind:class="[movement.type === 'in' ? 'green_dark' : 'red_warning', 'movements__amount']">
            {{ Number(movement.amount).toFixed(2) }}
            <span>{{ account.coin.toUpperCase() }}</span>
          </span>
          <span class="movements__balance">
            {{ formatAmount(movement.newBalance)}}
            <span>{{ account.coin.toUpperCase() }}</span>
          </span>
        </li>
      </ul>
    </div>
  </div>

</template>

<script>
  import Vue from "vue"
  import Transfer from "./Transfer.vue"

  export default {
    name: 'wallet',
    data() {
      return {
        showTransferPanel: false,
        account: {
          id: 1,
          address: '9hqxmYPYq2DQ31gasR1smJTnUvwYCyokM9FA7t858JnD8WJFiKJ',
          alias: 'Antoñito',
          coin: 'radix',
          balance: 1524.00,
          movements: [
            {
              id: 1,
              timestamp: '1535212164244',
              type: 'in',
              amount: 100.01,
              address: '9fbzfeeMaMkjKZcRLHkukFP8VffbQBsxNpivjjH4b5oZyASCLuC',
              alias: 'My long lost friend Timothy Bangers The Third',
            },
            {
              id: 2,
              timestamp: '1535212206867',
              type: 'out',
              amount: 10.37,
              address: '9gUVQUT5TvYQA3v8cLgHC15T7uN6beB88mbTJ2SdkbDqbSEX5jc',
              alias: 'Robert',
            },
            {
              id: 3,
              timestamp: '1535212212696',
              type: 'out',
              amount: 5.15,
              address: '9i15dpSxi89V1LMK1WeUiwEnGo9eDJHaxdtoAhr5uDACHGBQzeT',
              alias: '',
            },
            {
              id: 4,
              timestamp: '1535212218839',
              type: 'in',
              amount: 33.00,
              address: '9griU558xVTxwxSoFvXNXojxgFwBKZGwLoEkDFYx7J7sS1MNXmj',
              alias: 'chatbot',
            },
            {
              id: 5,
              timestamp: '1535212227855',
              type: 'out',
              amount: 11.55,
              address: '9iQSiCbNTGpMYoRfoTtNLMaYqxNwsw7BXUSzFxXDLSRxNoL2GuE',
              alias: 'Carmack',
            }
          ]
        }
      }
    },
    props: {
      contacts: {
        type: Array,
        default: () => []
      }
    },
    methods: {
      // Get the movement date
      getDate(timestamp) {
        const date = new Date(timestamp * 1)
        const months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec']
        // At some point years should be used
        const year = `${date.getFullYear()}`
        const month = months[date.getMonth()]
        const day = date.getDate()
        const formattedDate = `${day} / ${month}`

        return formattedDate
      },
      // Get the movement time
      getTime(timestamp) {
        const date = new Date(timestamp * 1)
        const hours = `0${date.getHours()}`
        const minutes = `0${date.getMinutes()}`
        const seconds = `0${date.getSeconds()}`
        const formattedTime = `${hours.substr(-2)}:${minutes.substr(-2)}:${seconds.substr(-2)}`

        return formattedTime
      },
      formatAmount(balance) {
        let balanceNumber = Number(balance)
        balanceNumber = balanceNumber.toFixed(2)

        return balanceNumber.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
      },
      addTransfer(amount, address, timestamp) {
        let movements = this.account.movements
        const newKey = Object.keys(movements).length
        const formattedAmount = this.formatAmount(amount)
        const newElement = {
          id: newKey,
          amount: formattedAmount,
          address: address,
          timestamp: timestamp,
          type: 'out',
        }
        Vue.set(movements, newKey, newElement)

        this.account.movements = movements
        this.account.balance = this.account.balance - amount
        this.handleMovements()
      },
      toggleTransferPanel() {
        this.showTransferPanel = !this.showTransferPanel
      },
      handleMovements() {
        // Before mounting we order the movements by timestamp
        // & we add the new balance after each movement
        // In prod this would be different
        const movements = this.account.movements
        const orderedMovements = movements.sort((a, b) => b.timestamp > a.timestamp)
        let balance = Number(this.account.balance)

        orderedMovements.map((move, index) => {
          const amount = index !== 0 ? Number(orderedMovements[index-1].amount) : Number(move.amount)
          const moveType = index !== 0 && orderedMovements[index-1].type
          const sumAmount = balance + amount
          const restAmount = balance - amount
          const newBalance = index !== 0 ? moveType === 'in' ? restAmount : sumAmount : balance

          balance = newBalance
          Vue.set( movements, index, {
            ...move,
            newBalance: balance
          })
        })

        this.account.movements = movements
      }
    },
    beforeMount() {
      this.handleMovements()
    },
    components: {
      Transfer,
    },
  }
</script>

<style lang="scss" scoped>
  @import '../assets/scss/wallet.scss'
</style>
