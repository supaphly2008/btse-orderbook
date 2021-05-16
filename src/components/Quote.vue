<template>
  <div class="quote">
    <table class="quote-table">
      <tr>
        <th>Price ({{ currency }})</th>
        <th>Size</th>
        <th>Total</th>
      </tr>
      <tr
        v-for="(d, index) in data"
        :key="index"
        :class="{
          flashBuy: flash(d, index, orderType.BUY),
          flashSell: flash(d, index, orderType.SELL),
        }"
      >
        <td :class="priceClass">{{ d.price }}</td>
        <td>{{ d.size }}</td>
        <td>{{ d.quoteTotal }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export const ORDER_TYPE = {
  BUY: "BUY",
  SELL: "SELL",
};

export default {
  name: "Quote",
  props: {
    type: {
      type: String,
      require: true,
    },
    data: {
      type: Array,
      default: () => [],
    },
    currency: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      oldVal: [],
      orderType: ORDER_TYPE,
    };
  },
  computed: {
    priceClass() {
      return this.type === ORDER_TYPE.BUY ? "buy-price" : "sell-price";
    },
  },
  methods: {
    flash(current, index, type) {
      return (
        this.type === type &&
        this.oldVal.length &&
        (current.price !== this.oldVal[index].price ||
          current.size !== this.oldVal[index].size)
      );
    },
  },
  watch: {
    data(val, oldVal) {
      this.oldVal = oldVal;
    },
  },
};
</script>

<style scoped>
.quote-table {
  text-align: right;
  border-collapse: collapse;
}
.quote-table tr > th {
  min-width: 100px;
}
.buy-price {
  color: #03b05c;
}
.sell-price {
  color: #ff5c59;
}

.buy-total {
  background-color: rgba(16, 186, 104, 0.12);
}

.flashSell {
  animation: flashRedBackground 0.3s;
}

.flashBuy {
  animation: flashGreenBackground 0.3s;
}

@keyframes flashRedBackground {
  0%,
  100% {
    background-color: transparent;
  }
  50% {
    background-color: #fdb8c4;
  }
}

@keyframes flashGreenBackground {
  0%,
  100% {
    background-color: transparent;
  }
  50% {
    background-color: #92f3c4;
  }
}
</style>