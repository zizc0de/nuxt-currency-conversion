<template>
  <div class="container">
    <div v-if="isSelectMode == false">
      <br />
      <br />
      <button @click="openChoice()" class="OpenSelect">
        + Add More Currency
      </button>
    </div>
    <div v-if="isSelectMode == true">
      <div class="dropdown">
        <button class="dropbtn">
          {{ this.selectedChoice }} - {{ this.selectedCurrencyName }}
        </button>
        <div class="dropdown-content">
          <div v-for="(value, index) of choiceList" :key="index">
            <button @click="setChoice(value)" class="dropdown-content-style">
              {{ value }}
            </button>
          </div>
        </div>
      </div>
      <br />
      <br />
      <br />
      <button @click="addDisplayCurrency()" class="OpenSelect">
        ADD
      </button>
    </div>
  </div>
</template>

<script>
import cc from "currency-codes";
export default {
  data() {
    return {
      isSelectMode: false,
      selectedChoice: "IDR",
      selectedCurrencyName: "Rupiah",
      displayCurrencyList: ["USD", "IDR"],
      choiceList: ["JPY", "EUR", "HKD"]
    };
  },
  methods: {
    addDisplayCurrency() {
      this.displayCurrencyList.push(this.selectedChoice);
      this.isSelectMode = false;
    },
    setChoice(data) {
      this.selectedChoice = data;
      this.selectedCurrencyName = cc.code(data)["currency"];
    },
    openChoice() {
      let data = this.choiceList[0];
      this.selectedChoice = data;
      this.selectedCurrencyName = cc.code(data)["currency"];
      this.isSelectMode = true;
    }
  }
};
</script>

<style scoped>
.container {
  display: block;
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

.OpenSelect {
  text-align: center;
  background-color: orange;
  color: white;
  display: inline-block;
  width: 90%;
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
  display: inline-block;
  width: 90%;
}

/* Dropdown Content (Hidden by Default) */
.dropdown-content-style {
  text-align: left;
  display: inline-block;
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
