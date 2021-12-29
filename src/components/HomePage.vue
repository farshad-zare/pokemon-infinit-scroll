<template>
  <h1 class="title">home page</h1>
  <div class="card-container" ref="el">
    <PokeCard
      v-for="(pokemon, index) in pokemons"
      :key="pokemon.name"
      :pokemon="pokemon"
      :pokemonIndex="index"
    />
  </div>
</template>

<script>
  import axios from "axios";

  import PokeCard from "./PokeCard.vue";

  export default {
    name: "home-component",
    components: { PokeCard },

    data() {
      return {
        pokemons: [],
        pokemonLimit: 25,
        currentPage: 0,
        loadingData: false,
      };
    },

    computed: {
      pokemonOffset() {
        const offset = this.currentPage * this.pokemonLimit;
        return offset;
      },
    },

    methods: {
      async getPokemons() {
        this.loadingData = true;
        const request = await axios.get(
          `https://pokeapi.co/api/v2/pokemon?offset=${this.pokemonOffset}&limit=${this.pokemonLimit}`
        );
        request.data.results.forEach((poke) => {
          this.pokemons.push(poke);
        });
        this.loadingData = false;
        this.currentPage += 1;
      },

      reachedBottom() {
        if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
          if (this.loadingData) {
            return;
          }
          this.getPokemons();
        }
      },
    },

    mounted() {
      this.getPokemons();
      window.addEventListener("scroll", this.reachedBottom);
    },
  };
</script>

<style scoped>
  .title {
    margin: 1rem auto;
    width: max-content;
  }

  .card-container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;

    padding-inline: 2rem;
    gap: 20px;
    margin-bottom: 2rem;
  }
</style>
