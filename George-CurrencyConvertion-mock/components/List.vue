<template>
  <div>
    <p v-if="$fetchState.pending">
      Fetching Exchange Rate...
    </p>
    <p v-else-if="$fetchState.error">
      Error while fetching Exchange Rate: {{ $fetchState.error.message }}
    </p>
    <ul v-else>
      <ul v-for="(item, index) in displayCurrencyList" :key="index">
          <br>
          <Row
            :current="current"
            :currency="item"
            :rate="data.rates[item]"
            :amount="amount"
            @emitRemoval="removeCurrency"
          />
        <br>
      </ul>
    </ul>
    <br>
    <br>
    <div v-if="choiceList.length > 0" class="dropdown">
      <button class="dropbtn">
        Add More
      </button>
      <div class="dropdown-content">
        <div v-for="(value, index) of choiceList" :key="index">
          <button @click="addDisplayCurrency(value)">
            {{ value }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Row from '~/components/row.vue'

export default {
  components: {
    Row
  },
  props: {
    current: {
      type: String,
      default: 'USD'
    },
    currencies: {
      type: Array,
      default: [] === ['USD']
    },
    amount: {
      type: Number,
      default: 1
    }
  },
  async fetch () {
    this.data = await this.$http.$get('currencyData.js')

    this.emitAvailableCurrencyListUpdate(this.data)
  },
  data () {
    return {
      data: [],
      displayCurrencyList: ['USD', 'IDR'],
      choiceList: []
    }
  },
  watch: {
    current (newVal, oldVal) {
      this.fetchAgain()
    },
    displayCurrencyList () {
      this.generateNewChoice()
    },
    currencies () {
      this.currencies.sort()
      this.generateNewChoice()
    }
  },
  methods: {
    emitAvailableCurrencyListUpdate (incomingData) {
      this.$emit('emitRawData', incomingData)
    },
    async fetchAgain () {
      this.data = await this.$http.$get('currencyData.js')
      this.emitAvailableCurrencyListUpdate(this.data)
    },
    generateNewChoice () {
      this.choiceList = this.currencies

      for (const key in this.displayCurrencyList) {
        const value = this.displayCurrencyList[key]

        this.choiceList = this.choiceList.filter(
          function (item, index) {
            return item !== value
          })
      }
    },
    removeCurrency (data) {
      this.displayCurrencyList = this.displayCurrencyList.filter(
        function (value, index) {
          return value !== data
        })
    },
    addDisplayCurrency (data) {
      this.displayCurrencyList.push(data)
    }
  }
}
</script>

<style lang="css" scoped>
.dropbtn {
  background-color: #4CAF50;
  color: white;
  padding: 16px;
  font-size: 16px;
  border: none;
  cursor: pointer;
}

/* The container <div> - needed to position the dropdown content */
.dropdown {
  position: relative;
  display: inline-block;
}

/* Dropdown Content (Hidden by Default) */
.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
  z-index: 1;
}

/* Links inside the dropdown */
.dropdown-content a {
  color: black;
  padding: 16px;
  text-align: center;
  text-decoration: none;
  display: block;
}

/* Change color of dropdown links on hover */
.dropdown-content a:hover {background-color: #f1f1f1}

/* Show the dropdown menu on hover */
.dropdown:hover .dropdown-content {
  display: block;
}

/* Change the background color of the dropdown button when the dropdown content is shown */
.dropdown:hover .dropbtn {
  background-color: #3e8e41;
}
</style>
