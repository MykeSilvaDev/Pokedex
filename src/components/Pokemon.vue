            /*  Aula 276 Componente Poke 06/05/23 18:09 
                Aula 277 Buscando mais informações 07/05/23 22:22
                Aula 278 Melhorando Componente 07/05/23 22:48 
                Aula 279 Trocando Sprite 07/05/23 23:50*/

// DIGITAR (VUE) PARA ELE SETAR A ESTRUTURA BÁSICA DE UM COMPONENTE

<template>
<!--(2-76) DEFININDO AS PROPS E FILTER (1-276)-->
    <div class="pokemon">
      
        
<!--(6-278 COLANDO O VIEW DO SITE DA BULMA-->
<div class="card">
  <div class="card-image">
    <figure class="">
<!--(11-279)ESTOU FAZENDO UM DATA BINDING, PASSANDO AS IMAGENS DOS POKEMONS (currentImg)-->
      <img :src="currentImg" alt="Placeholder image">
    </figure>
  </div>
  <div class="card-content">
    <div class="media">
     
      <div class="media-content">
        <p class="title is-4">{{num}} {{name}} {{upper}}</p>
<!--ESTOU PEGANDO O NOME DA VARIÁVEL THIS (4-277) NÃO PRECISA USAR NO THIS NO HTML-->
        <p class="subtitle is-6">{{ pokemon.type }}</p>
      </div>
    </div>

<!--(12-279) DEFININDO O BOTÃO MUDAR SPRITE -->
    <div class="content">
<!--CHAMANDO O EVENTO MUDAR SPRITE-->
        <button class="button is-medium is-fullwidth" @click="mudarSprite">Mudar Sprite</button>
    </div>
  </div>
</div>

    </div>
</template>

<script>
/*(3-277) IMPORTANDO O AXIOS */
import axios from 'axios';


/*(1-276) */
export default {

/*(4-277) FAZER UMA REQUISIÇÃO COM O AXIOS */
    created: function(){
// ESTOU PASSANDO A URL DE CADA POKEMON
        axios.get(this.url).then(res => {
// ABRINDO O CONSOLE ELE ME RETORNA VÁRIOS OBJETOS 
            console.log(res.data);
// ESSAS INFORMAÇÕES ESTÃO NO ARRAY DE CADA POKEMON QUE EU QUERO EXIBIR
            this.pokemon.type = res.data.types[0].type.name;
            this.pokemon.front = res.data.sprites.front_default;
            this.pokemon.back = res.data.sprites.back_default;

/*(10-279) ASSIM QUE A REQUISIÇÃO ACONTECER EU VOU DIZER QUE A CURRENT IMG (a imagem atual dele já é a imagem de frente)*/
            this.currentImg = this.pokemon.front;
            console.log(this.pokemon);
        })
    },

/*(5-277) CRIANDO O MÉTODO DATA */
    data(){
// VAI ME RETORNAR UM OBJETO
        return{

/*(9-279) CRIANDO DUAS VARIÁVEIS PARA MOSTRAR FRENTE E VERSO DOS POKEMONS */
            isFront: true,
            currentImg: '',

/*(7-278) PARA TORNAR AS PROPRIEDADES REATIVAS (pokemon type, front e back) PARA QUE POSSAM APARECER NO HTML PRECISO 
COLOCÁ-LOS DENTRO DO DATA  */
            pokemon: {
                type: '',
                front: '',
                back: ''
            }
        }
    },

    props: {
        num: Number,
        name: String,
        url: String
    },
// DEFININDO OS FILTROS
    filters: {
        upper: function(value){
// ESTOU PEGANDO A PRIMEIRA LETRA DA STRING E DEFININDO COMO MAIÚSCULA 
            var newName = value[0].toUpperCase() + value.slice(1); // -> slice eu pego todo o caracter e deleto o primeiro
            return newName;
        }
    },
/*(13-279) QUANDO EU CLICAR NO BOTÃO MUDAR SPRITE, EU QUERO DISPARAR UM EVENTO */
    methods: {
        mudarSprite: function(){
// SE O SPRITE ATUAL É DE FRENTE, QUANDO EU CHAMAR ESSE MÉTODO EU VOU INVERTER
            if(this.isFront){
                this.isFront = false;
                this.currentImg = this.pokemon.back;
// SE NO MOMENTO QUE EU TOCAR ESSE BOTÃO O SPRITE ESTIVER DE COSTAS 
            }else{
                this.isFront = true;
                this.currentImg = this.pokemon.front;
            }
        }
    }
}
</script>

<style>
/*(8-278) ADICIONANDO MARGIN EM CADA CARD DO MEU POKEMON */
    .pokemon{
        margin-top: 2%;
    }
</style>