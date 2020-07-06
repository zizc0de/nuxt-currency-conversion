<template>
  <div class="bar">
    <p>{{ this.selectedCurrency }} - {{ this.selectedCurrencyName }}</p>
    <br />
    <div class="container">
      <div class="seperator" />
      <div class="dropdown">
        <button class="dropbtn">
          {{ selectedCurrency }}
          <i class="arrow down"></i>
        </button>
        <div class="dropdown-content">
          <div v-for="(value, index) of currencyDataList" :key="index">
            <button
              @click="updateSelectedCurrency(value)"
              class="dropdown-content-style"
            >
              {{ value }}
            </button>
          </div>
        </div>
      </div>
      <div class="seperator" />
      <input
        v-model="userInput"
        placeholder="1"
        name="userInput"
        type="number"
        class="amount-input-field"
      />
      <div class="seperator" />
    </div>
  </div>
</template>

<script>
import cc from "currency-codes";

export default {
  data() {
    return {
      currencyDataList: ["USD", "IDR"],
      userInput: 1,
      selectedCurrency: "USD",
      selectedCurrencyName: "US Dollar"
    };
  },
  methods: {
    updateSelectedCurrency(data) {
      this.selectedCurrency = data;
      this.selectedCurrencyName = cc.code(data)["currency"];
    }
  }
};
</script>

<style scoped>
.bar {
  padding: 8px;
}

.container {
  display: flex;
  align-items: center;
  text-align: center;
  justify-content: space-evenly;
}

.seperator {
  width: 32px;
}

.arrow {
  border: solid black;
  border-width: 0 3px 3px 0;
  display: inline-block;
  padding: 3px;
}

.down {
  transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
}

.amount-input-field {
  text-align: right;
  display: inline-block;
  width: 100%;
  padding-top: 8px;
  padding-bottom: 8px;
}

.dropbtn {
  text-align: left;
  cursor: pointer;
  display: inline-block;
  width: 100%;
  padding-top: 8px;
  padding-bottom: 8px;
  text-overflow: ellipsis;
  overflow: hidden;
}

/* The container <div> - needed to position the dropdown content */
.dropdown {
  position: relative;
  display: block;
  width: 100%;
}

/* Dropdown Content (Hidden by Default) */
.dropdown-content-style {
  text-align: left;
  display: block;
  width: 100%;
}

.dropdown-content {
  display: none;
  text-align: left;
  position: absolute;
  background-color: #f9f9f9;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  width: 100%;
  z-index: 1;
}

/* Links inside the dropdown */
.dropdown-content a {
  color: black;
  text-align: left;
  text-decoration: none;
  display: block;
}

/* Change color of dropdown links on hover */
.dropdown-content a:hover {
  background-color: #f1f1f1;
}

/* Show the dropdown menu on hover */
.dropdown:hover .dropdown-content {
  display: block;
}

/* Change the background color of the dropdown button when the dropdown content is shown */
.dropdown:hover .dropbtn {
  background-color: #3e8e41;
  color: white;
}
</style>
