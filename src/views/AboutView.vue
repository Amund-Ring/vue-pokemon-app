<template>
  <div class="about bg-slate-400 flex justify-center p-14">
    <div
      v-if="pokemon"
      class="w-80 p-20 flex flex-col items-center justify-center rounded-md border-8 border-emerald-400 bg-purple-200 drop-shadow-2xl"
    >
      <h3 class="text-2xl uppercase text-green-900">{{ pokemon.name }}</h3>
      <div class="flex justify-center my-8">
        <img class="w-56" :src="pokemon.sprites.front_shiny" alt="" />
        <img class="w-56" :src="pokemon.sprites.back_shiny" alt="" />
      </div>
      <h3 class="uppercase text-yellow-600">Types</h3>
      <div v-for="(type, idx) in pokemon.types" :key="idx">
        <h5 class="uppercase text-blue-800">{{ type.type.name }}</h5>
      </div>
    </div>
  </div>
</template>

<script>
  import { useRoute } from 'vue-router';
  import { reactive, toRefs } from 'vue';

  export default {
    setup() {
      const route = useRoute();
      const state = reactive({
        pokemon: null
      });

      fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
        .then(res => res.json())
        .then(data => {
          state.pokemon = data;
        });

      return { ...toRefs(state) };
    }
  };
</script>
