<script>
export default {
  data() {
    return {
      coins: [],
      filterCoins: [],
      titles: ["#", "Coin", "Price", "Price Change", "24h Volume"],
      textSearch: "",
    };
  },

  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false&locale=en"
    );
    const data = await res.json();
    console.log(data);
    this.coins = data;
    this.filterCoins = data;
  },

  methods: {
    searchCoin() {
      this.filterCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
};
</script>

<template>
  <div class="container">
    <div class="row">
      <h1 class="text-center">Coin Market</h1>

      <input
        type="text"
        class="form-control bg-dark text-light rounded-0 border-0 my-4"
        placeholder="Search Coin"
        @keyup="searchCoin()"
        v-model="textSearch"
      />

      <table class="table table-dark">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">
              {{ title }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filterCoins" :key="coin.id">
            <td class="text-secondary">
              {{ index + 1 }}
            </td>
            <td>
              <img :src="coin.image" style="width: 2rem" class="me-2" alt="" />
              <span>
                {{ coin.name }}
              </span>
              <span class="ms-2 text-uppercase text-secondary">
                {{ coin.symbol }}
              </span>
            </td>
            <td>${{ coin.current_price }}</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              {{ coin.price_change_percentage_24h }}%
            </td>
            <td>${{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
