<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <input
        class="input is-rounded"
        type="text"
        placeholder="Buscar pokémon pelo nome"
        v-model="busca"
      />
      <button
        class="button is-fullwidth is-success"
        id="buscaBtn"
        @click="buscar"
      >
        Buscar
      </button>
    </div>
    <div
      class="column is-half is-offset-one-quarter"
      v-for="poke in filteredPokemons"
      :key="poke.url"
    >
      <Pokemon
        :name="poke.name"
        :url="poke.url"
        :num="poke.url.split('/')[6]"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";
export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: "",
    };
  },
  created: function() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;
      });
  },
  components: {
    Pokemon,
  },
  methods: {
    buscar: function() {
      if (this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter((pokemon) =>
          pokemon.name.includes(this.busca.toLowerCase())
        );
      }
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

#buscaBtn {
  margin-top: 15px;
}
</style>
