<template>  
<main :style="'--c1: ' + styleBasedOnType[0] + '; ' + '--c2: ' + styleBasedOnType[1]">
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
      selectedPokemon: 1,
      pokemon: {
        name: '',
        imageUrl: '',
        types: [],
      },
      userGuess: '',
      hidden: true,
    };
  },

  mounted () {
    axios
    /* Get Pokemon Data */
      .get('https://pokeapi.co/api/v2/pokemon/80')
      .then(response => {
        this.pokemon.name = response.data.name;
        this.pokemon.imageUrl = response.data.sprites.front_default;
        response.data.types.forEach(e => this.pokemon.types.push(e.type.name));

        })
      
  },

  computed: {
    styleBasedOnType: function () {
      let temp = [];
      for(const type in this.pokemon.types) {
        switch(this.pokemon.types[type]) {
          case 'normal': 
            temp.push('#A8A878');
          break;
          case 'fire': 
            temp.push('#d88332');
          break;
          case 'water': 
            temp.push('#6299d6');
          break;
          case 'grass': 
            temp.push('#80cbae');
          break;
          case 'electric': 
            temp.push('#e7cf61');
          break;
          case 'ice': 
            temp.push('#98D8D8');
          break;
          case 'fighting': 
            temp.push('#C03028');
          break;
          case 'poison': 
            temp.push('#8a51d2');
          break;
          case 'ground': 
            temp.push('#9f6825');
          break;
          case 'flying': 
            temp.push('#ffffff');
          break;
          case 'psychic': 
            temp.push('#F85888');
          break;
          case 'bug': 
            temp.push('#62a666');
          break;
          case 'rock': 
            temp.push('#B8A038');
          break;
          case 'ghost': 
            temp.push('#340f6c');
          break;
          case 'dark': 
            temp.push('#1c0d31');
          break;
          case 'dragon': 
            temp.push('#96c6fa');
          break;
          case 'steel': 
            temp.push('#B8B8D0');
          break;
          case 'fairy': 
            temp.push('#EE99AC');
          break;
          default:
            temp.push('#ffffff');
        }
      }
      //Linear gradient in CSS requires 2 properties. We duplicate the first color if there isn't a second color.
      if (temp.length < 2) {
        temp.push(temp[0]);
      }
      return temp;
    }
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
    --pokeball-border: 20px;
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
    background-color: #fff;
  }
  main:before {
    content: "";
    position: absolute;
    top: 0;
    z-index: 1;
    width: 100vw;
    height: calc(50vh - var(--pokeball-border));
    background-color: var(--bg);
    background-image: linear-gradient(var(--c1), var(--c2)), url(assets/pokeball-outline.png), url(assets/pokeball-outline.png);
    background-position: 0 0, calc(0% - 250px) 0, calc(100% + 250px)  0;
    background-size: cover, 500px, 500px;
    background-blend-mode: overlay;
    background-repeat: no-repeat;
    border-bottom: var(--pokeball-border) solid #000;
  }
  .pokeball-center {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color: #fff;
    border: 20px solid #000;
    border-radius: 50%;
    width: 300px;
    height: 300px;
    position: relative;
    z-index: 100;
  }
  .pokemon-image {
    min-width: 300px;
    min-height: 300px;
    background-repeat: no-repeat;
    background-size: contain;
  }
  .pokemon-image--hidden {
    filter: brightness(0%);
    animation: transition 3s ease-in forwards;
  }

  @keyframes transition {
    0% {
    filter: brightness(0%) blur(100px);
    }

    100% {
    filter: brightness(0%) blur(0px);
    }
  }
</style>
