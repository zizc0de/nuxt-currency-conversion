<template>
  <div class="rowContained">
    <div class="container">
      <div class="rowContained">
        <div class="container">
          <div class="topLeft">
            <h4>{{ currency }}</h4>
          </div>
          <div class="topRight">
            <h4>{{ rate * amount }}</h4>
          </div>
        </div>
        {{ currency }} - {{ currencyName }}
        <br>
        1 {{ current }} = {{ currency }} {{ rate }}
      </div>
      <div class="removeButton">
        <button @click="removeCurrency">
          X
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import cc from "currency-codes";
export default {
  props: {
    current: {
      type: String,
      default: "USD"
    },
    currency: {
      type: String,
      default: "USD"
    },
    rate: {
      type: Number,
      default: 1
    },
    amount: {
      type: Number,
      default: 1
    }
  },
  data() {
    return {
      currencyName: cc.code(this.currency)["currency"]
    };
  },
  methods: {
    removeCurrency() {
      this.$emit("emitRemoval", this.currency);
    }
  }
};
</script>

<style lang="css" scoped>
.container {
  width: 100%;
  display: flex;
  align-items: left;
  text-align: left;
}

.rowContained {
  width: 100%;
  display: flexbox;
}

.topLeft {
  width: 100%;
  display: flexbox;
  align-items: left;
  text-align: left;
}

.topRight {
  width: 100%;
  align-items: right;
  text-align: right;
}

.innerRowContainer {
  overflow: hidden;
  width: 2000px;
}

.removeButton {
  float: right;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  padding: 4pt;
}
.leftBox {
  float: left;
  align-items: flex-start;
  justify-content: left;
  text-align: left;
}
.rightBox {
  float: right;
  align-items: flex-end;
  justify-content: right;
  text-align: right;
}
</style>
