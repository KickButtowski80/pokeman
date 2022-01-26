<template>
  <div>
    <div class="w-full flex justify-center">
      <input
        type="text"
        name=""
        id=""
        v-model="pokemonName"
        placeholder="Enter Pokmon here"
        class="mt-10 p-2 border-blue-500 border-2"
      />
    </div>
    <div class="mt-10 p-4 flex flex-wrap justify-center">
      <div
        class="ml-4 text-2x text-blue-400"
        v-for="(pokemon, idx) in filteredPokemon"
        :key="idx"
      >
        <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
          {{ pokemon.name }}
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { reactive, toRefs, computed } from "vue";
export default {
  name: "Home",
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      pokemonName: "",
      filteredPokemon: computed(() => updatePokemon()),
    });

    function updatePokemon() {
      if (!state.pokemonName) return state.pokemons;
      const foundPokemon = state.pokemons.filter((p) =>
        p.name.includes(state.pokemonName)
      );
      return foundPokemon;
    }

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      .then((res) => res.json())
      .then((data) => {
        console.log(data.results);
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
          {}
        );
      });
    //  https://stackoverflow.com/questions/66585688/what-is-the-difference-between-ref-toref-and-torefs
    return { ...toRefs(state) };
  },
};
</script>
