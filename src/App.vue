<template>
  <div id="app">
    <h2>BTSE Order book</h2>
    <h4>{{ sourceData.symbol }}</h4>
    <Orderbook :data="sourceData" v-if="sourceData" />
  </div>
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
          args: ["orderBookL2Api:BTC-USDT_0"],
        })
      );
    };

    ws.onmessage = (event) => {
      const data = JSON.parse(event.data);
      this.sourceData = data.data;
    };

    ws.onclose = () => {
      console.log("websocket is closed");
    };

    ws.onerror = (err) => {
      console.log("connection error: ", err);
    };
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
