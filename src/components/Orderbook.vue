<template>
  <div class="orderbook">
    <Quote
      :currency="currency"
      :data="sellQuoteData"
      :type="orderType.SELL"
      v-if="sellQuoteData"
    />
    <Quote
      :currency="currency"
      :data="buyQuoteData"
      :type="orderType.BUY"
      v-if="buyQuoteData"
    />
  </div>
</template>

<script>
import Big from "big.js";
import Quote, { ORDER_TYPE } from "./Quote";
const QUOTE_SIZE = 12;

export default {
  name: "Orderbook",
  components: {
    Quote,
  },
  props: {
    data: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      orderType: ORDER_TYPE,
    };
  },
  computed: {
    currency() {
      return this.data.currency || "";
    },
    buyQuoteData() {
      if (!Object.keys(this.data).length) {
        return;
      }
      // return price, size and total
      let total = 0;
      return this.data.buyQuote.slice(0, QUOTE_SIZE).map((q) => {
        const result = {
          ...q,
          quoteTotal: Big(q.size).plus(total).toFixed(4),
        };
        total += Number(q.size);
        return result;
      });
    },
    sellQuoteData() {
      if (!Object.keys(this.data).length) {
        return;
      }
      // return price, size and total
      let total = 0;
      return this.data.sellQuote
        .slice(0, QUOTE_SIZE)
        .reverse()
        .map((q) => {
          const result = {
            ...q,
            quoteTotal: Big(q.size).plus(total).toFixed(4),
          };
          total += Number(q.size);
          return result;
        })
        .reverse();
    },
  },
};
</script>