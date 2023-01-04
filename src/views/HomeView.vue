<template>
  <div class="flex w-full justify-center">
    <input
      type="text"
      placeholder="Enter Pokemon here"
      class="mt-10 rounded-md border-2 border-blue-500 p-2 text-center"
      v-model="text"
    />
  </div>

  <div class="mt-10 flex flex-wrap justify-center p-4">
    <div
      class="text-2x m-4 ml-4 rounded-md border-2 border-red-200 p-4 text-blue-400"
      v-for="(pokemon, idx) in filteredPokemons"
      :key="idx"
    >
      <!-- <RouterLink :to="`/about/${urlIdLookup[pokemon.name]}`"> -->
      <RouterLink :to="`/about/${urlIdLookup[pokemon.name]}`">
        {{ pokemon.name }}
      </RouterLink>
    </div>
  </div>

  <div class="home flex flex-col items-center">
    <!-- <h3>Hello World</h3> -->
    <!-- {{ pokemons }} -->
  </div>
</template>

<script>
  // @ is an alias to /src
  import { reactive, toRefs, computed } from 'vue';

  export default {
    name: 'HomeView',
    setup() {
      const state = reactive({
        pokemons: [],
        urlIdLookup: {},
        text: '',
        filteredPokemons: computed(() => updatePokemon())
      });

      function updatePokemon() {
        if (!state.text) {
          return state.pokemons;
        }

        return state.pokemons.filter(pokemon =>
          pokemon.name.includes(state.text.toLowerCase())
        );
      }

      fetch('https://pokeapi.co/api/v2/pokemon?limit=100&offset=0')
        .then(res => res.json())
        .then(data => {
          // console.log(data.results);
          state.pokemons = data.results;
          state.urlIdLookup = data.results.reduce(
            (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
            {}
          );
        });

      return { ...toRefs(state) };
    }
  };
</script>
