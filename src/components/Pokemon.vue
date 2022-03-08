<template>
    <div class="card">
        <img :src="this.currentImage">
        <p class="p-title">{{this.index}} - {{namePokemon}}</p>
        <p class="p-type">{{this.pokemon.type}}</p>
        <button @click="rotateImage">Girar Imagem</button>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data(){
        return{
            currentImage: '',
            turnBackImage: false,
            pokemon: {
                type: '',
                frontImage: '',
                backImage: ''
            }
        }
    },
    created: function(){
        axios.get(this.url).then( res => {
            this.pokemon.type = res.data.types[0].type.name;
            this.pokemon.frontImage = res.data.sprites.front_default;
            this.pokemon.backImage = res.data.sprites.back_default; 
            this.currentImage = this.pokemon.frontImage; 
            this.pokemon.type = this.pokemon.type[0].toUpperCase() + this.pokemon.type.slice(1)
        })
    },
    props: {
        name: String,
        index: Number,
        url: String
    },
    computed: {
        namePokemon: function(){
            return this.name[0].toUpperCase() + this.name.slice(1)
        },
    },
    methods: {
        rotateImage: function() {
            if(this.turnBackImage == false){
                this.turnBackImage = true;
                this.currentImage = this.pokemon.backImage;
            } else {
                this.turnBackImage = false;
                this.currentImage = this.pokemon.frontImage;  
            }
        }
    }

}
</script>

<style scoped>
    .card {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        border: 1px solid rgb(242, 241, 241);
        background-color: rgb(249, 249, 249);
        border-radius: 15px;
        width: 400px;
        height: 250px;
        margin: 10px 10px;
        box-shadow: rgb(230, 230, 230) 1px 1px 3px 1px;
    }
    .p-title {
        margin: 0;
        margin-top: 5px;
        font-size: 1.3em;
    }
    .p-type {
        margin: 10px 0;
        font-size: 1em;
    }
    button {
        border: none;
        outline: none;
        border-radius: 5px;
        color: black;
        background-color: rgb(211, 211, 211);
        padding: 8px;
    }

    button:hover {
        color: black;
        background-color: rgb(150, 150, 150);       
    }
</style>