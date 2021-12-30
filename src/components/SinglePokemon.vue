<template>
  <div class="container" v-if="pokemonData">
    <div class="next">
      <router-link :to="`${parseInt(pokemonID) + 1}`">next </router-link>
    </div>
    <div class="prev">
      <router-link :to="`${parseInt(pokemonID) - 1}`">previous </router-link>
    </div>
    <div class="pokemon-img">
      <img :src="imgUrl" />
    </div>
    <div class="details">
      <h2>{{ pokemonData.name }}</h2>
      <div class="wrapper">
        <span>weight: {{ pokemonData.weight }}</span>
        <span>height: {{ pokemonData.height }}</span>
      </div>
    </div>
    <div class="img-container" v-if="pokemonImages">
      <img v-for="img in pokemonImages" :src="img" :key="img" alt="" />
    </div>
  </div>

  <div class="spinner" v-else>loading</div>
</template>

<script>
  import axios from "axios";

  export default {
    name: "pokemon",
    data: () => {
      return {
        pokemonData: null,
        loadingData: false,
      };
    },

    computed: {
      pokemonID() {
        return this.$route.params.id;
      },

      imgUrl() {
        return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${this.pokemonID}.png`;
      },

      pokemonImages() {
        return this.extractStrFromObj(this.pokemonData.sprites);
      },
    },

    methods: {
      async getPokemonData() {
        this.loadingData = true;
        const fetchData = await axios.get(
          `https://pokeapi.co/api/v2/pokemon/${this.pokemonID}`
        );
        this.pokemonData = fetchData.data;
        console.log(this.pokemonData);
        this.loadingData = false;
      },

      extractStrFromObj(obj) {
        if (!obj) {
          return;
        }
        let strings = [];
        Object.values(obj).forEach((val) => {
          if (typeof val === "string") {
            strings.push(val);
          } else {
            this.extractStrFromObj(val);
          }
        });
        return strings;
      },
    },

    created() {
      this.getPokemonData();
    },

    watch: {
      pokemonID: function () {
        this.getPokemonData();
      },
    },
  };
</script>

<style scoped>
  .container {
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;

    position: relative;
    max-width: 45rem;
  }

  .pokemon-img img {
    aspect-ratio: 1;
    width: 200px;
    margin: 0 auto;
  }

  .details {
    flex-basis: 100%;

    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    flex-wrap: wrap;
  }

  .img-container {
    width: 100%;

    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
  }

  span:first-of-type {
    margin-right: 1rem;
  }

  .prev {
    position: absolute;
    left: 20px;
  }

  .next {
    position: absolute;
    right: 20px;
  }
</style>
