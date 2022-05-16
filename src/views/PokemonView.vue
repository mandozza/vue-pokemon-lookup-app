<template>
  <div class="pokemon">
    <div>to search fo another pokemon click the link above</div>
     <div v-if="pokemon"
      className="w-3/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center"
    >
      <h3 className="text-2xl text-green-900 uppercase">{{ pokemon.name }}</h3>
      <div class="flex justify-center">
        <img className="w-50" :src="pokemon.sprites.front_default" alt="" />
      </div>
      <h3  class="text-yellow-400">Types</h3>
      <div v-for="(type, idx) in pokemon.types" :key="idx">
        <h5 class="text-blue-900">{{type.type.name}}</h5>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs } from "vue";
import { useRoute } from "vue-router";
export default {
  setup() {
    const route = useRoute();
    const pokemonDetails = reactive({
      pokemon: null
    });
    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
      .then((res) => res.json())
      .then((data) => {
        pokemonDetails.pokemon = data;
        console.log(data)
      });
    return { ...toRefs(pokemonDetails) };
  }
};
</script>
