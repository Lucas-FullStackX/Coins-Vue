<template>
  <div>
    <h1>Coin Market</h1>
    <input
      type="text"
      class="search"
      placeholder="Search Coin"
      @keyup="searchCoin()"
      v-model="textSearch"
    />
    <div class="box">
      <div class="main">
        <span>#</span>
        <h3>Name</h3>
        <span>Price</span>
        <span>24h</span>
        <span>Volumen</span>
      </div>
      <div v-for="(coin, index) in filteredCoins" :key="coin.id" class="row">
        <span class="index">{{ index + 1 }}</span>
        <img :src="coin.image" />
        <h3>{{ coin.name }}</h3>
        <span>{{ coin.symbol.toUpperCase() }}</span>
        <span>${{ coin.current_price }}</span>
        <span
          :class="
            coin.price_change_percentage_24h > 0 ? 'positive' : 'negative'
          "
        >
          {{ coin.price_change_percentage_24h }}%</span
        >
        <span> {{ coin.total_volume.toLocaleString() }}%</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: {},
      textSearch: "",
    };
  },
  async mounted() {
    const response = await axios.get(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    this.coins = response.data;
    this.filteredCoins = response.data;
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
  components: {},
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
body {
  background-color: rgb(20, 20, 20);
}
body::-webkit-scrollbar {
  width: 10px;
  background-color: rgb(20, 20, 20);
}
body::-webkit-scrollbar-thumb {
  background: rgb(255, 255, 255);
  border-radius: 10px;
  height: 30%;
}
h1 {
  color: #fff;
}
.box {
  margin: auto;
  width: 99%;
  border-radius: 15px;
  background-color: rgba(0, 0, 0, 0.281);
  padding: 3px;
}
.search {
  border: 2px solid #161616;
  height: 40px;
  width: 90%;
  border-radius: 15px;
  padding: 0 20px;
  font-size: 20px;
}
.row {
  background-color: #343434;
  margin: 5px;
  border-radius: 20px;
  display: grid;
  grid-template-columns: 20px 90px repeat(5, 10%);
  justify-content: space-between;
}
.main {
  background-color: #ff000000;
  margin: 5px;
  border-radius: 20px;
  display: grid;
  grid-template-columns: 5% 45% 20% 15% 15%;
}
.row img {
  height: 80px;
  margin: 5px 0;
  min-width: 5%;
}
h3 {
  color: white;
  align-self: center;
  min-width: 10%;
}
.symbol {
  min-width: 3%;
  color: #fff;
}
.index {
  margin-left: 25px;
}
span {
  color: #fff;
  align-self: center;
}
.negative {
  color: rgb(255, 65, 65);
}
.positive {
  color: rgb(57, 226, 0);
}
</style>
