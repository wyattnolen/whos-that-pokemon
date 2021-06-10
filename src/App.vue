<template>  
<main :style="'--c1: ' + styleBasedOnType[0] + '; ' + '--c2: ' + styleBasedOnType[1]">
  <section class="pokeball-center">
    <div
      class="pokemon-image"
      :class="hidden ? 'pokemon-image--hidden' : '' "
      :style="'background-image: url(' + pokemon.imageUrl + ')'">
    </div>
  </section>

  <section class="pokemon-userInput">
    <input @keyup.enter="checkGuess" v-model="userGuess" class="userInput" placeholder="Pokemon Name">
    <button @click="checkGuess" class="button button--primary"><span>Submit</span></button>
    <button @click="newPokemon" :class="{ 'button--loading': loading }" class="button button--secondary button--refresh"></button>
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
        id: 80,
        name: '',
        imageUrl: '',
        types: [],
      },
      userGuess: '',
      hidden: true,
      loading: false,
    };
  },

  mounted () {
    this.getPokemon();
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
    getPokemon() {
      this.loading = true;

      axios
      .get(`https://pokeapi.co/api/v2/pokemon/${this.pokemon.id}`)
      .then(response => {
        
        this.pokemon.name = response.data.name;
        this.pokemon.imageUrl = response.data.sprites.front_default;

        let tempArray = [];
        response.data.types.forEach(e => tempArray.push(e.type.name));
        this.pokemon.types = tempArray;
        })
      .finally(() => (this.loading = false))
    },

    normalizeUserGuess() {
      return this.userGuess.toLowerCase().trim();
    },

    checkGuess() {
      if (this.normalizeUserGuess() === this.pokemon.name) {
        this.hidden = false;
      }
    },

    clearGuess() {
      this.userGuess = "";
    },

    hidePokemon() {
      this.hidden = true;
    },

    getRandomPokemon() {
      return Math.floor(Math.random()*150);
    },

    newPokemon() {
      this.pokemon.id = this.getRandomPokemon();
      this.clearGuess();
      this.hidePokemon();
      this.getPokemon();
    },
  }
}
</script>

<style>
  :root {
    --pokeball-border: 20px;
    --inputSize: 36px;
    --inputRadius: 4px;
  }
  * {
    box-sizing: border-box;
  }
  html, body {
    margin: 0;
    padding: 0;
  }
  main {
    display: grid;
    grid-template-rows: 63vh;
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
    align-self: flex-end;
    justify-self: center;
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
  }

  .pokemon-userInput {
    display: grid;
    grid-template-areas:"a a" 
                        "b c";
    grid-gap: 10px;
    max-width: 500px;
    margin-top: 7vh;
    margin-left: auto;
    margin-right: auto;
    align-self: flex-start;

  }
  .pokemon-userInput :nth-child(1) {
    grid-area: a;
  }
  .pokemon-userInput :nth-child(2) {
    grid-area: b;
  }
  .pokemon-userInput :nth-child(3) {
    grid-area: c;
  }

  .userInput {
    font-size: 1rem;
    font-weight: 400;
    letter-spacing: .009375em;
    align-self: flex-end;
    width: 100%;
    min-width: 64px;
    height: var(--inputSize);
    padding: 12px 16px 14px;
    transition: opacity 150ms cubic-bezier(0.4, 0, 0.2, 1);
    border: 1px solid rgba(0,0,0,.5);
    border-radius: var(--inputRadius);
    outline: 0;
    background: none;
    appearance: none;
  }

  .userInput:hover, .userInput:focus {
    border-color: rgba(0,0,0,.8);
  }

  .button {
    font-size: .875rem;
    line-height: 2.25rem;
    font-weight: 500;
    letter-spacing: .0892857143em;
    text-decoration: none;
    text-transform: uppercase;
    padding: 0 8px 0 8px;
    display: inline-flex;
    position: relative;
    align-items: center;
    justify-content: center;
    height: var(--inputSize);
    min-width: 64px;
    border: 1px solid transparent;
    border-radius: var(--inputRadius);
    outline: none;
    line-height: inherit;
    appearance: none;
    overflow: visible;
    vertical-align: middle;
    cursor: pointer;
  }

  .button:hover {
    filter: brightness(0.95);
  }

  .button:active {
    filter: brightness(0.9);    
  }

  .button--primary {
    background-color: #ce615a; 
    border-color: #ce615a;
    color: #fff;
  }

  .button--secondary {
    background-color: #fff;
    border-color: #ce615a;
    color: #ce615a;
  }

  .button--refresh:before {
    content: "";
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    background: url(assets/refresh-icon.png);
    background-repeat: no-repeat;
    background-size: 24px;
    background-position: center center;
  }

  .button--loading:before {
    animation: loading 3s linear infinite;
  }

  @keyframes loading {
    from {
      transform: rotate(0deg);
    }    
    to {
      transform: rotate(-360deg);
    }
  }

</style>
