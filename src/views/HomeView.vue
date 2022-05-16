<template>
  <div class="w-full flex justify-center">
    <input type="text" class="w-1/2 p-2 border border-blue-100 rounded-lg" v-model="search"  @keyup.enter="updateSearch"/>
  </div>
  <div class="w-3/5 flex flex-wrap justify-center mt-6">
    <span  v-for="(pokemon, index) in filteredPokemons" :key="index" class="p-2 border border-blue-100 bg-slate-500 text-cyan-400 rounded-lg m-2">
      <router-link :to="'/pokemon/' + pokemon.name">{{ pokemon.name }}</router-link>
    </span>
  </div>
</template>

<script>
// @ is an alias to /src
import { reactive, toRefs, computed } from "vue";

export default {
  setup() {
    const state = reactive({
      pokemons: [],
      pokemonIds: {},
      search: "",
      filteredPokemons: computed(() => filterResults())
      })

    // grab the pokemon data from the api.
    // preloads the data into the state only loading top 1000 pokemon.
    fetch("https://pokeapi.co/api/v2/pokemon?limit=1000&offset=0")
      .then((res) => res.json())
      .then((data) => {
        state.pokemons = data.results;
        // Loop through the pokemon array and get the id from the url.
        // Then store the id and name in the PokemonIds object.
        // ### this makes no sense REMOVE THIS ###
        state.pokemonIds = data.results.reduce((accumulator, curVal, index) =>
          accumulator = {...accumulator, [curVal.name]:index + 1}
        , {})
      })

    function filterResults() {
      if(!state.search) return []

      return state.pokemons.filter(pokemon => {
        return pokemon.name.toLowerCase().includes(state.search.toLowerCase());
      })
    }
      // toRefs is a helper function that converts the state object to a ref object.
      return {
        ...toRefs(state)
      }
  },
  methods: {
    updateSearch(e) {
      console.log ('searchquery', e.target.value)
      //console.log ('filteredPokemons', this.filteredPokemons)
      if(this.filteredPokemons.length === 0) {
        console.log('no results')
      }
    }
  }
};
</script>
