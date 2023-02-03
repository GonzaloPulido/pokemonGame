<template>

    <h1 v-if="!pokemon">Cargando ...</h1>
    <div v-else="pokemon">
      <h3>Puntos: {{ puntos }}</h3>
      <h3>Vidas: {{ vidas }}</h3>
      <h1>¿Cual es este Pokemon?</h1>
      
      <PokemonPictures :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
      <PokemonOptions :pokemons="pokemonsArr" @selection="checkAnswer" v-if="!showAnswer"/>
      <template v-if="showPokemon">
        <h2 class="fade-in">{{ gameOver }}</h2>
        <h2 class="fade-in">{{ message }}</h2>
        <button @click="newGame">Jugar de nuevo</button>
      </template>
    </div>
  </template>
  
  <script>

  import PokemonOptions from '@/components/PokemonOptions.vue';
  import PokemonPictures from '@/components/PokemonPictures.vue';
  import getPokemonOptions from '@/helpers/getPokemonOptions.js';




  export default { components: {PokemonOptions,PokemonPictures},
  data() {
    return {
      pokemon: null,
      pokemonsArr: [],
      showPokemon: false,
      showAnswer: false,
      message: "",
      gameOver: "",
      puntos: 0,
      vidas: 3,
    }
  },
  methods: {
    async mixedPokemons() {
      this.pokemonsArr = await getPokemonOptions()
      const randomPokemon = Math.floor(Math.random()*4)
      this.pokemon = this.pokemonsArr[randomPokemon]
    },
    checkAnswer(pokemonId) {
      this.showAnswer = true
      this.showPokemon = true

      if(pokemonId === this.pokemon.id){ 
        this.puntos ++
        this.message = `¡¡¡¡¡ CORRECTO !!!!!`
      }else {
        this.vidas --
        if (this.vidas === 0){
          this.gameOver = `Has perdido todas tus vidas`
          this.puntos = 0
          this.vidas = 3
        }
        this.message = `Fallaste, el pokemon era ${this.pokemon.name}`
      }
    },
    newGame(){
      this.pokemon = null,
      this.pokemonsArr = [],
      this.showPokemon = false,
      this.showAnswer = false,
      this.mixedPokemons()
    }
  },
  mounted(){
    this.mixedPokemons()
  }

}
  </script>

  <style>
    #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    }
  </style>
  