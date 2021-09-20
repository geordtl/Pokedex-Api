<template>
	<div id="app">
		<div class="column is-half is-offset-one-quarter">
		<img src="./assets/pokemon.png" id="pokemon-logo" alt="pokemon-logo">
		<hr>
		<h4 class="is-size-4">Pokedéx</h4>
		<input type="text" placeholder="Buscar pókemon pelo nome" class="input is-rounded" v-model="busca">
		<button class="button is-fullwidth button is-primary" id="busca-btn" @click="buscar">Buscar</button>

			<div v-for="(poke, index) in filteredPokemons" :key="poke.url">
				<Pokemon :name="poke.name" :url="poke.url" :num="index+1"/>
			</div>
		</div>
		
	</div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';
export default {
	name: "App",
	data(){
		return {
			pokemons: [],
			filteredPokemons: [],
			busca: ''
		}
	},
	created: function() {
		axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res => {
			console.log('Pegou a lista de Pokemon!');
			this.pokemons = res.data.results; //array de resultados para a var pokemons
			this.filteredPokemons = res.data.results; 
		})
	},
	components: {
		Pokemon,
	},
	methods: {
		buscar: function(){
			this.filteredPokemons = this.pokemons;
			if(this.busca == '' || this.busca == ' '){
				this.filteredPokemons = this.pokemons;
			}else {
				this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.includes(this.busca.toLowerCase()));
		 	}
		 	}
		}
	}
	// computed: {
		// resultadoBusca: function() {
		// 	if(this.busca == '' || this.busca == ' '){
		// 		return this.pokemons;
		// 	}else {
		// 		return this.pokemons.filter(pokemon => pokemon.name == this.busca)
		// 	}
		// }
	// }
</script>

<style lang="scss">
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}

#pokemon-logo {
	width: 450px;
}

#busca-btn {
	margin-top: 2%;
}
</style>
