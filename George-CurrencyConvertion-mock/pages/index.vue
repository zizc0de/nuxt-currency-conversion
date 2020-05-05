<template>
  <div class="global-sizing">
    <Navbar />
    <CountryAmountPicker />
    <list
      :current="selectedCurrency"
      :currencies="currencyDataList"
      :amount="userInput"
      @emitRawData="updateCurrencyDataList"
    />
    <CurrencyAdder />
  </div>
</template>

<script>
import List from "~/components/List.vue";
import Navbar from "~/components/Navbar.vue";
import CountryAmountPicker from "~/components/CountryAmountPicker.vue";
import CurrencyAdder from "~/components/CurrencyAdder.vue";

export default {
  components: {
    List,
    Navbar,
    CountryAmountPicker,
    CurrencyAdder
  },
  data() {
    return {
      data: [],
      currencyDataList: ["USD", "IDR"],
      userInput: 1,
      selectedCurrency: "USD"
    };
  },
  methods: {
    updateCurrencyDataList(incomingData) {
      for (const key in incomingData.rates) {
        if (!this.currencyDataList.includes(key)) {
          this.currencyDataList.push(key);
        }
      }
      this.currencyDataList.sort();
    },
    updateSelectedCurrency(data) {
      this.selectedCurrency = data;
    }
  }
};
</script>

<style lang="css" scoped></style>
