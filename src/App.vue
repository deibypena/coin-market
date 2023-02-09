<script setup>
  import { ref } from 'vue'

  let coins = ref([])
  let filteredCoins = ref([])
  let titles = [ "#", "coin", "Price", "Price Change", "24h Volume" ]
  let textSearch = ''
  
  const  traerData = async () => {
    const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const data = await res.json()
    coins.value = data
    filteredCoins.value = data
  } 

  traerData()

  const searchCoin = () => {
    filteredCoins.value = coins.value.filter((coin) => 
      coin.name.toLowerCase().includes(textSearch.toLocaleLowerCase()) ||
      coin.symbol.toLowerCase().includes(textSearch.toLocaleLowerCase())
    )

  }

</script>

<template> 

  <div class="py-4 px-6 bg-zinc-800 h-screen overflow-y-auto">
    <h1 class="text-white text-3xl text-center font-bold mt-4 mb-10">Coin market</h1>
    <div class="container mx-auto mb-10">
      <input type="text" class="bg-zinc-600 w-full p-4 text-xl text-white font-medium rounded-md" placeholder="Search Coin" @keyup="searchCoin()" v-model="textSearch">
    </div>
    <table class="w-full">
      <thead>
        <tr class="bg-zinc-800 text-white text-xl border border-b-2">
          <th class="py-4" v-for="title in titles" :key="title">
            {{ title }}
          </th>
        </tr>
      </thead>

      <tbody class="bg-zinc-700 text-white">
        <tr class="text-center border border-b-1 border-zinc-500" v-for="(coin, index) in filteredCoins" 
          :key="coin.id">
          <td class="text-zinc-500">
            {{ index + 1 }}
          </td>
          <td class="py-4 flex pl-5 items-center gap-2">
            <img class="w-10 h-10" :src="coin.image">
            <span>
              {{ coin.name }}
            </span>
            <span class="text-zinc-500">
              {{ coin.symbol }}
            </span>
          </td>
          <td>
            $ {{ coin.current_price }}
          </td>
          <td :class="[coin.price_change_percentage_24h > 0 ? 'text-green-500' : 'text-red-500']">
            {{ coin.price_change_percentage_24h }} %
          </td>
          <td>
            $ {{ coin.total_volume.toLocaleString() }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  
  

</template>

<style>
</style>