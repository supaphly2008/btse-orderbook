<template>
  <div class="quote">
    <table class="quote-table">
      <tr>
        <th>Price ({{ currency }})</th>
        <th>Size</th>
        <th>Total</th>
      </tr>
      <tr
        class="quote-row"
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
        <div class="tooltip-text">
          <div>Average Price: {{ averagePrice }}</div>
          <div>Total Value: {{ (d.price * d.size).toFixed(2) }}</div>
        </div>
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
    quoteSize: {
      type: Number,
      require: true,
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
    averagePrice() {
      let total = 0;
      this.data.forEach((d) => {
        total += Number(d.price);
      });

      return (total / this.quoteSize).toFixed(2);
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

.quote-row {
  cursor: pointer;
}

.tooltip-text {
  background-color: #000;
  color: #fff;
  text-align: center;
  border-radius: 6px;
  padding: 5px 10px;
  position: absolute;
  z-index: 1;
  display: none;
  margin-left: 10px;
  text-align: left;
}

.quote-row:hover .tooltip-text {
  animation: fadeIn 1s;
  display: block;
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

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
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