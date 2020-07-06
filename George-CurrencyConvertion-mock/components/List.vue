<template>
  <div class="container">
    <p v-if="$fetchState.pending">
      Fetching Exchange Rate...
    </p>
    <p v-else-if="$fetchState.error">
      Error while fetching Exchange Rate: {{ $fetchState.error.message }}
    </p>
    <div v-else v-for="(item, index) in displayCurrencyList" :key="index">
      <Row
        :current="current"
        :currency="item"
        :rate="data.rates[item]"
        :amount="amount"
        @emitRemoval="removeCurrency"
      />
    </div>
  </div>
</template>

<script>
import Row from "~/components/row.vue";

export default {
  components: {
    Row
  },
  props: {
    current: {
      type: String,
      default: "USD"
    },
    currencies: {
      type: Array,
      default: [] === ["USD"]
    },
    amount: {
      type: Number,
      default: 1
    }
  },
  async fetch() {
    this.data = await this.$http.$get("currencyData.js");

    this.emitAvailableCurrencyListUpdate(this.data);
  },
  data() {
    return {
      data: [],
      displayCurrencyList: ["USD", "IDR"],
      choiceList: []
    };
  },
  watch: {
    current(newVal, oldVal) {
      this.fetchAgain();
    },
    displayCurrencyList() {
      this.generateNewChoice();
    },
    currencies() {
      this.currencies.sort();
      this.generateNewChoice();
    }
  },
  methods: {
    emitAvailableCurrencyListUpdate(incomingData) {
      this.$emit("emitRawData", incomingData);
    },
    async fetchAgain() {
      this.data = await this.$http.$get("currencyData.js");
      this.emitAvailableCurrencyListUpdate(this.data);
    },
    generateNewChoice() {
      this.choiceList = this.currencies;

      for (const key in this.displayCurrencyList) {
        const value = this.displayCurrencyList[key];

        this.choiceList = this.choiceList.filter(function(item, index) {
          return item !== value;
        });
      }
    },
    removeCurrency(data) {
      this.displayCurrencyList = this.displayCurrencyList.filter(function(
        value,
        index
      ) {
        return value !== data;
      });
    },
    addDisplayCurrency(data) {
      this.displayCurrencyList.push(data);
    }
  }
};
</script>

<style lang="css" scoped>
.container {
  width: 100%;
  display: block;
  align-items: center;
  text-align: center;
  justify-content: space-evenly;
  padding-left: 16px;
  padding-right: 16px;
}
</style>
