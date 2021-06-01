<template>  
<main :class="pokemon.types[0] + ' ' +  pokemon.types[1] ">
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
        imageUrl: '',
        types: []
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
        this.pokemon.imageUrl = response.data.sprites.front_default;
        response.data.types.forEach(e => this.pokemon.types.push(e.type.name));
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
    background: linear-gradient(to top, 
    #fff, 
    #fff 50vh, 
    #000 50vh, 
    #000 calc(50vh + 20px), 
    var(--bg) calc(50vh + 20px), 
    var(--bg));
  }
  main:before {
    content: "";
    position: absolute;
    top: 0;
    z-index: 1;
    width: 100vw;
    height: calc(50vh - var(--pokeball-border));
    background-color: var(--bg);
    background-image: linear-gradient(var(--bg), var(--bg2)), url(assets/pokeball-outline.png), url(assets/pokeball-outline.png);
    background-position: 0 0, calc(0% - 250px) 0, calc(100% + 250px)  0;
    background-size: cover, 500px, 500px;
    background-blend-mode: overlay;
    background-repeat: no-repeat;
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
  }

.normal {
  --bg: #ffffff;
  --bg2: #ffffff;
  }
.fire {
  --bg: #db8633;
  --bg2: #db8633;
  }
.water {
  --bg: #6299d6;
  --bg2: #6299d6;
  }
.grass {
  --bg: #80cbae;
  --bg2: #80cbae;
  }
.electric {
  --bg: #e7cf61;
  --bg2: #e7cf61;
  }
.ice {
  --bg: #ffffff;
  --bg2: #ffffff;
  }
.fighting {
  --bg: #ffffff;
  --bg2: #ffffff;
  }
.poison {
  --bg: #8a51d2;
  --bg2: #8a51d2;
  }
.ground {
  --bg: #ffffff;
  --bg2: #ffffff;
  }
.flying {
  --bg: #ffffff;
  --bg2: #ffffff;
  }
.psychic {
  --bg: #ffffff;
  --bg2: #ffffff;
  }
.bug {
  --bg: #62a666;
  --bg2: #62a666;
  }
.rock {
  --bg: #ffffff;
  --bg2: #ffffff;
  }
.ghost {
  --bg: #340f6c;
  --bg2: #340f6c;
  }
.dark {
  --bg: #1c0d31;
  --bg2: #1c0d31;
  }
.dragon {
  --bg: #96c6fa;
  --bg2: #96c6fa;
  }
.steel {
  --bg: #ffffff;
  --bg2: #ffffff;
  }
.fairy {
  --bg: #ffffff;
  --bg2: #ffffff;
  } 

</style>
