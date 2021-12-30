<template>
  <router-link :to="pokemonDest" class="pokemon-card">
    <img class="pokemon-img" :src="imgUrl" />

    <h3>{{ pokemon.name }}</h3>
  </router-link>
</template>

<script>
  export default {
    name: "poke-card",
    props: ["pokemon", "pokemonIndex"],

    computed: {
      imgUrl() {
        return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${this.pokemon.index}.png`;
      },

      pokemonDest() {
        return `/pokemon/${this.pokemon.index}`;
      },
    },
  };
</script>

<style scoped>
  h3 {
    color: red;
  }

  .pokemon-card {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    background-color: bisque;
  }

  .pokemon-img {
    aspect-ratio: 1;
    width: 170px;
  }
</style>
