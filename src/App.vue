<template>
  <div id="app"><Orderbook :data="sourceData" v-if="this.sourceData" /></div>
</template>

<script>
const BTSE_SPOT_WEBSOCKET_URL = "wss://ws.btse.com/ws/spot";
import Orderbook from "./components/Orderbook";

export default {
  name: "App",
  components: {
    Orderbook,
  },
  data() {
    return {
      sourceData: {},
    };
  },
  created() {
    const ws = new WebSocket(BTSE_SPOT_WEBSOCKET_URL);

    ws.onopen = () => {
      ws.send(
        JSON.stringify({
          op: "subscribe",
          args: ["orderBookL2Api:ETH-USDT_0"],
        })
      );
    };

    ws.onmessage = (event) => {
      const data = JSON.parse(event.data);
      this.sourceData = data.data;
      // console.log(this.sourceData);
    };

    ws.onclose = () => {
      console.log("websocket is closed");
    };

    ws.onerror = (err) => {
      console.log("connection error: ", err);
    };
  },
  watch: {
    buyQuote(val) {
      console.log("asdf", val);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
