<template>

<div id="app">
  <h1>Favorites</h1>
  <div v-for="(coin,n) in favorite_coins" :key='n'>
    <p>
    <span class="coin">{{coin}}</span>
    <router-link :to='coin' tag="button" class = 'button'>See Asset</router-link>
    <button @click="removeCoin(n)" class = 'button'> Remove</button>
    </p>
  </div>

  <p>
    <select v-model="newCoin">
      <option disabled value="">Please select</option>
      <option v-for='(coin,n) in coins_library' v-bind:value='coin' :key = 'n'>
        {{coin}}
      </option>
    </select>
    <button @click="addCoin" class = ' button' >Add to favorites</button>
  </p>

</div>

</template>

<script>

export default {
  name: 'favorites',
  data() {
    return {
      favorite_coins:[],
      newCoin: null,
      coins_library: ['etherium', 'litecoin', 'bitcoincash', 'bitcoin']
      }
  },

  mounted() {

    if(localStorage.getItem('favorite_coins')) {
      try {
        this.favorite_coins = JSON.parse(localStorage.getItem('favorite_coins'));
      } catch(e) {
        localStorage.removeItem('favorite_coins');
      }
    }
  },

  methods: {
    addCoin() {
      // ensure they actually typed something
      if(!this.newCoin) return;
      this.favorite_coins.push(this.newCoin);
      this.newCoin = '';
      this.saveCoin();
    },
    removeCoin(x) {
      this.favorite_coins.splice(x,1);
      this.saveCoin();
    },
    saveCoin() {
      let parsed = JSON.stringify(this.favorite_coins);
      localStorage.setItem('favorite_coins', parsed);
    }
  }

}

</script>

<style>
.coin {
  padding-right: 10px
}

.button {
  border-radius: 10px;
  margin-left: 10px
}
</style>
