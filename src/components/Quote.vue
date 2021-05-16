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
          flashBuy:
            type === orderType.BUY &&
            oldVal.length &&
            (d.price !== oldVal[index].price || d.size !== oldVal[index].size),
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

.flashBuy {
  animation: flashGreenBackground 0.3s;
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