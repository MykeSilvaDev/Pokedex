/*                            Aula 274 Importando Bibliotecas 
                              Aula 275 Requisições HTTP 
                              Aula 276 Componente Poke 
                              Aula 278 Melhorando Componente 
                              Aula 280 Importando Imagens 
                              Aula 281 Sistema de Busca */

<template>


  

  <div id="app">

    

<!--(8-278) EU QUERO QUE MINHA COLUNA ENGLOBE TODAS AS LISTAS DE POKEMONS (COM A CLASE column is-half) ELA VAI 
SER O TAMANHO DA MINHA TELA /  A CLASSE (is-offset-one-quarter) EU VOU MOVER A MINHA COLUNA 1/4 A MINHA DIREITA -->
    <div class="column is-half is-offset-one-quarter">

<!--(9-280) VOU CARREGAR O LOGO --> 
      <img src="./assets/Pokédex_logo.png" alt="">
      <hr>
      <!--O TAMANHO DO TEXTO VAI ATÉ 7-->
      <h4 class="is-size-4">Pokedex</h4>

<!--(10-281) -->
      <input type="text" name="" id="" placeholder="Buscar pokemon pelo nome" v-model="busca" class="input is-rounded">
<!--VAI TER UM EVENTO DE CLICK COM O MÉTODO BUSCAR-->
      <button class="button is-medium is-fullwidth is-success" id="buscaBtn" @click="buscar">Buscar</button>

<!--(4-276) pokemons -> busca por arrays-->
<!--(17-281) FILTRAR O SISTEMA DE BUSCA PELO (filteredPokemons) -->
    <div v-for="(poke,index) in filteredPokemons" :key="poke.url">
<!--ESTOU FAZENDO A INTERPOLAÇÃO PARA SER O ID DE CADA POKEMON E O NOME DE CADA UM DELE (name) ESTA NO SITE DA POKEDEX-->

<!--(7-276) IMPORTANDO COMPONENTE DO POKEMON E PASSANDO AS PROPS (arquivo Pokemon.vue (1-276))-->
      <Pokemon :name="poke.name" :url="poke.url" :num="index+1" />
    </div>


    </div>
  </div>
</template>


<script>

/*(5-276) IMPORTANDO O COMPONENTE (POKEMONE.VUE) */
import Pokemon from './components/Pokemon';

/*(1-274) Importando axios */
import axios from 'axios';
export default {
  name: 'App',
//(3-275) CRIANDO UMA VARIÁVEL (DATA)
  data(){
    return{
      pokemons: [],

/*(14-281) ARRAY DE POKEMONS FILTRADOS */
    filteredPokemons: [],

/*(11-281) CRIANDO A VARIÁVEL DE BUSCA */
      busca: ''
    }
  },

/*                          [ COMPONENTE AXIOS ] */

/*(2-275) MÉTODO CREATE VAI RECEBER UMA FUNÇÃO QUANDO ELE FOR CHAMADO*/
  created: function(){
    //console.log(axios) // ESTOU CHAMANDO SÓ PARA NÃO DAR ERRO
/*ESTOU PASSANDO O MÉTOD GET PARA PEGAR DADOS, E ESTOU PASSANDO A URL QUE ESPECIFA NA PÁG NO POKEDEX PARA FAZER A REQUISIÇÃO */
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
// ASSIM QUE ESSA FUNÇÃO ACABAR, PASSE OS DADOS PARA A FUNÇÃO THEN
    .then(res =>{
// ESTOU USANDO OQUE ELE ME RETORNA NO CONSOLE.LOG (ARRAY)
      console.log(res.data.results);
      console.log("Pegou a Lista de Pokemons")
      this.pokemons = res.data.results;

/*(15-281) FILTRAR OS POKEMONS*/
      this.filteredPokemons = res.data.results;

      console.log("teste");
    })

  },

/* (6-276)                       [ COMPONENTE POKEMON ] */
components: {
  Pokemon
},

/*(16-281) VOU CRIAR UM MÉTODO DE BUSCA */
  methods:{
/*TODA VEZ QUE O MÉTODO BUSCAR FOR CHAMADO ATRÁVES DE UM EVENTO VOU DIZER QUE THIS.FILTEREDPOKEMON RECEBE
THIS.POKEMONS EU ESTOU RESETANDO A LISTA DE POKEMONS FILTRADOS  */
    buscar: function(){
      this.filteredPokemons = this.pokemons;
/*VOU DEFINIR SE THIS BUSCA É UMA STRING VAZIA OU UMA STRING COM ESPAÇO (se o nome que eu digitar na busca for vazio )
vou retornar o array da forma que ele é*/  
    if(this.busca == '' || this.busca == ' '){
      this.filteredPokemons = this.pokemons;
/*SE A MINHA BARRA DE BUSCA NÃO ESTA COM O TEXTO VAZIO SIGNIFICA QUE EU DIGITEI ALGUMA COISA, (significa que vou retornar
os pokemons que correspondem a regra (pokemon.name))*/     
    }else{
/*aqui vou filtrar os pokemons pelo nome */
      this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);
    }
  }
},

/*(13-281) CRIANDO O SISTEMA DE BUSCAS */
computed: {
  
/*
  resultadoBusca: function(){
VOU DEFINIR SE THIS BUSCA É UMA STRING VAZIA OU UMA STRING COM ESPAÇO (se o nome que eu digitar na busca for vazio )
vou retornar o array da forma que ele é  
    if(this.busca == '' || this.busca == ' '){
      return this.pokemons;
/*SE A MINHA BARRA DE BUSCA NÃO ESTA COM O TEXTO VAZIO SIGNIFICA QUE EU DIGITEI ALGUMA COISA, (significa que vou retornar
os pokemons que correspondem a regra (pokemon.name))     
    }else{
      return this.pokemons.filter(pokemon => pokemon.name == this.busca)
    }
  }
*/

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

/*(12-281) FORMATANDO OS BOTÕES */
#buscaBtn{
  margin-top: 2%;
}

</style>
