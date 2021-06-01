<template>  
  <HelloWorld msg="Welcome to Your Vue.js App"/>
  <h2>{{pokemon.name}}</h2>
  <div
    class="pokemon-image"
    :class="hidden ? 'pokemon-image--hidden' : '' "
    :style="'background-image: url(' + pokemon.imageUrl + ')'">
  </div>
  <div>
    <input @keyup.enter="checkGuess" v-model="userGuess">
    <button @click="checkGuess">Submit</button>
  </div>

</template>

<script>
import axios from 'axios';
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
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
