<template>
  <div id="app">
    <h2>Pokedex</h2>
    <input type="text" v-model="search" placeholder="Search">
    <div class="pokedex">
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :index="index + 1" :name="poke.name" :url="poke.url"/>
      </div>  
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Pokemon from './components/Pokemon.vue'

export default {
  name: 'App',
  data() {
    return {
      pokemons: [],
      search: ''
    }
  },
  created: function(){
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then( res => {
      this.pokemons = res.data.results;
    })
  },
  components: {
    Pokemon,
  },
  computed: {
    filteredPokemons: function(){
      return this.pokemons.filter( (pokemons) => {
        return pokemons.name.match(this.search.toLowerCase())
      })
    }
  }
}
</script>

<style scoped>
  #app {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }  

  .pokedex {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center; 
  }

  h2 {
    color: rgb(82, 82, 82);
    font-size: 1.6em;
    border-bottom: 1px solid black;
  }

  input {
    border: 1px solid rgb(238, 238, 238);
    border-radius: 500px;
    outline: none;
    width: 390px;
    height: 20px;
    padding-left: 10px;
    margin-bottom: 10px;
  }

  input:focus {
    border: 1px solid rgb(145, 145, 145); 
  }
</style>
