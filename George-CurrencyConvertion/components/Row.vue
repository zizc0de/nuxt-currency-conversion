<template>
  <div class="rowContained">
    <div class="leftBox">
      {{ currency }}
      <br>
      1 {{ current }} = {{ currency }} {{ rate }}
    </div>
    <div class="removeButton">
      <button @click="removeCurrency">
        X
      </button>
    </div>
    <div class="rightBox">
      {{ rate * amount }}
    </div>
    <br>
  </div>
</template>

<script>
export default {
  props: {
    current: {
      type: String,
      default: 'USD'
    },
    currency: {
      type: String,
      default: 'USD'
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
  methods: {
    removeCurrency () {
      this.$emit('emitRemoval', this.currency)
    }
  }
}
</script>

<style lang="css" scoped>
.rowContained {
    width: 100%;
    display: flexbox;
    justify-content: center;
    align-items: center;
    text-align: center;
}
.innerRowContainer {
  overflow: hidden;
  width: 2000px;
}

.removeButton {
  float:right;
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
