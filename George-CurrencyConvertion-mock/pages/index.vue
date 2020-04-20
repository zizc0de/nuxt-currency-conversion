<template>
  <div class="global-sizing">
    <h1>Currency Convertion</h1>
    <p>Convert Currency</p>
    <div>
      <div class="dropdown">
        <button class="dropbtn">
          {{ selectedCurrency }}
        </button>
        <div class="dropdown-content">
          <div v-for="(value, index) of currencyDataList" :key="index">
            <button @click="updateSelectedCurrency(value)">
              {{ value }}
            </button>
          </div>
        </div>
      </div>
      <div>
        <input v-model="userInput" placeholder="1" name="userInput" type="number">
      </div>
    </div>
    <list
      :current="selectedCurrency"
      :currencies="currencyDataList"
      :amount="userInput"
      @emitRawData="updateCurrencyDataList"
    />
  </div>
</template>

<script>
import List from '~/components/List.vue'
export default {
  components: {
    List
  },
  data () {
    return {
      data: [],
      currencyDataList: ['USD', 'IDR'],
      userInput: 1,
      selectedCurrency: 'USD'
    }
  },
  methods: {
    updateCurrencyDataList (incomingData) {
      for (const key in incomingData.rates) {
        if (!this.currencyDataList.includes(key)) {
          this.currencyDataList.push(key)
        }
      }
      this.currencyDataList.sort()
    },
    updateSelectedCurrency (data) {
      this.selectedCurrency = data
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
