<!---------------------------------------------------------------
| @author Isac Canedo
---------------------------------------------------------------->
<template>
  <div id="app">    
    <h1>Preços do Bitcoin</h1>

  <section v-if="errored">
    <p>Pedimos desculpas, não estamos conseguindo recuperar as informações no momento. Por favor, tente novamente mais tarde.</p>
  </section>

  <section v-else>
    <div v-if="loading">Carregando...</div>

    <div
      v-else
      v-for="currency in info" v-bind:key="currency"
      class="currency">
      {{ currency.description }}:
      <span class="lighten">
        <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
      </span>
    </div>

  </section>
  </div>
</template>

<!--
Temos várias maneiras de recuperarmos informações de uma API, 
mas primeiro é interessante descobrir o formato dos dados, 
para sabermos o que mostraremos. 
Para fazer isso, faremos uma requisição para o endpoint da API, 
para podermos ver os dados. Podemos ver na documentação da API CoinDesk 
que esta chamada será feita para https://api.coindesk.com/v1/bpi/currentprice.json. 
Assim, criaremos uma propriedade de dados que eventualmente abrigará nossa informação e, então, 
recuperaremos os dados e atribuiremos usando o gatilho de ciclo de vida mounted:
-->

<script>
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      info: null,
      loading: true,
      errored: false
    }
  },
  filters: {
    currencydecimal (value) {
      return value.toFixed(2)
    }
  },
  mounted () {
    axios
      .get('https://api.coindesk.com/v1/bpi/currentprice.json')
      .then(response => {
        this.info = response.data.bpi
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  }
 
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /**margin-top: 60px;**/
}
</style>
