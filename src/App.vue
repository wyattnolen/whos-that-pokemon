<template>  
<main>
  <section class="pokeball-center">
    <div
      class="pokemon-image"
      :class="hidden ? 'pokemon-image--hidden' : '' "
      :style="'background-image: url(' + pokemon.imageUrl + ')'">
    </div>
  </section>

  <section>
    <input @keyup.enter="checkGuess" v-model="userGuess">
    <button @click="checkGuess">Submit</button>
  </section>
</main>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {
    
  },
  data () {
    return {
      pokemon: {
        name: '',
        imageUrl: ''
      },
      userGuess: '',
      hidden: true,
    };
  },
  mounted () {
    axios
    /* Get Pokemon Data */
      .get('https://pokeapi.co/api/v2/pokemon/1')
      .then(response => {
        this.pokemon.name = response.data.name;
        this.pokemon.imageUrl = response.data.sprites.front_default
        })
      
  },
  computed: {
  },
  methods: {
    normalizeUserGuess() {
      return this.userGuess.toLowerCase().trim();
    },

    checkGuess() {
      if (this.normalizeUserGuess() === this.pokemon.name) {
        this.hidden = false;
      }
    }
  }
}
</script>

<style>
  :root {

  }
  * {
    box-sizing: border-box;
  }
  html, body {
    margin: 0;
    padding: 0;
  }
  main {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    width: 100vw;
    height: 100vh;
    background-color: #6299d6;
    background: linear-gradient(to top, 
    #fff, 
    #fff 50vh, 
    #000 50vh, 
    #000 calc(50vh + 20px), 
    #6299d6 calc(50vh + 20px), 
    #6299d6);
  }
  main:before {
    content: "";
    position: absolute;
    z-index: -1;
    width: 100vw;
    height: 100vh;
    background-image: url(assets/pokeball-outline.png);
    background-blend-mode: screen;
  }
  .pokeball-center {
    background-color: #fff;
    border: 20px solid #000;
    border-radius: 50%;
    width: 300px;
    height: 300px;
    
  
  }
  .pokemon-image {
    min-width: 300px;
    min-height: 300px;
    background-repeat: no-repeat;
    background-size: contain;
  }
  .pokemon-image--hidden {
    /* filter: sepia() saturate(10000%) hue-rotate(180deg); */
    filter: brightness(0%);
  }
</style>
