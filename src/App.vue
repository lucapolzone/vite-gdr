<script>
import axios from "axios";
import { store } from './store';

export default {
  data() {
    return {
      api: store.api,
      title: "Card VS Card",
      characters: [],
      userCard: null,
      pcCard: null,
      userCardStrength: null,
      pcCardDefence: null,
      result: null,
    };
  },

  methods: {
    fetchCharacters() {
      axios.get(`${this.api.baseUrl}/api/characters`).then((res) => {
        this.characters = res.data.data;
      });
    },

    fetchCharacter(characterId, player) {
      axios.get(`${this.api.baseUrl}/api/characters/${characterId}`)
        .then((res) =>{
          if (player === 'player') {
            this.userCard = res.data;
          } else {
            this.pcCard = res.data;
          }
        })
        .catch((err) =>{
          console.error(err)
        });
    },

    random() {
      const randomId = Math.ceil(Math.random()*this.characters.length)
      return randomId;
    },

    handleStart(player) {
      // il player può essere 'player' | 'pc'
      const randNum = this.random();
      this.fetchCharacter(randNum, player);
    },

    fight(){
      this.userCardStrength = this.userCard.strength;
      this.pcCardDefence = this.pcCard.defence;

      if( this.userCardStrength > this.pcCardDefence ){
        console.log(this.userCardStrength);
        console.log(this.pcCardDefence);
        alert("HAI VINTO");
        
      } else if ( this.userCardStrength < this.pcCardDefence ) {
        console.log(this.userCardStrength);
        console.log(this.pcCardDefence);
        alert("PC HA VINTO");

      } else {
        console.log(this.userCardStrength);
        console.log(this.pcCardDefence);
        alert("PAREGGIO");
      }

    }
  },

  mounted() {
    this.fetchCharacters();
  },
};
</script>

<template>
  <div class="container">
    <h1 class="text-center">{{ title }}</h1>
    
    <!-- BOTTONE -->
    <div class="text-center my-2">
      <button class="btn btn-success me-5" @click="handleStart('player')">PESCA UNA CARTA!</button>
      <button class="btn btn-warning" @click="handleStart('pc')">PESCA LA CARTA DEL PC!</button>
    </div>

    <!-- CARD -->
    <div class="row g-3 d-flex justify-content-center align-items-center">
      <div class="col-5 d-flex flex-column align-items-center">
        <h4>USER</h4>
        <div v-if="userCard" class="card border-0 bg-transparent w-100">
          <div class="card-header p-0">
            <h5 class="card-title text-center fs-1">{{ userCard.name }}</h5>
          </div>
          <div class="card-body p-0">            
           <div class="image">
            <img :src="api.baseUrl + userCard.type.image" class="w-100" alt="">
           </div>
            <ul class="list-group list-group-flush">
              <li class="list-group-item p-0 bg-secondary ">
                <strong class="fs-4">Attacco: {{ userCard.strength }}</strong>
              </li>
              <li class="list-group-item p-0 bg-body-secondary">
                <strong class="fs-4">Difesa: {{ userCard.defence }}</strong>
              </li>
              <li class="list-group-item p-0 bg-secondary">
                <strong class="fs-4">Velocità: {{ userCard.speed }}</strong>
              </li>
              <li class="list-group-item p-0 bg-body-secondary">
                <strong class="fs-4">Vita: {{ userCard.life }}</strong>
              </li>
            </ul>
          </div>
        </div>

        <div v-else>
          <p>nessuna carta scelta</p>
        </div>
      </div>

      <div class="col-2">
        <div class="text-center">
          <button @click="fight()" class="text-center btn btn-danger">FIGHT!</button>
        </div>
      </div>

      <div class="col-5 d-flex flex-column align-items-center">
        <h4>COMPUTER</h4>
        <div v-if="pcCard" class="card border-0 bg-transparent w-100">
          <div class="card-header p-0">
            <h5 class="card-title text-center fs-1">{{ pcCard.name }}</h5>
          </div>
          <div class="card-body p-0">
            <div class="image">
              <img :src="api.baseUrl + pcCard.type.image" class="w-100" alt="">
            </div>
            <ul class="list-group list-group-flush">
              <li class="list-group-item p-0 bg-secondary">
                <strong class="fs-4">Attacco: {{ pcCard.strength }}</strong>
              </li>
              <li class="list-group-item p-0 bg-body-secondary">
                <strong class="fs-4">Difesa: {{ pcCard.defence }}</strong>
              </li>
              <li class="list-group-item p-0 bg-secondary">
                <strong class="fs-4">Velocità: {{ pcCard.speed }}</strong>
              </li>
              <li class="list-group-item p-0 bg-body-secondary">
                <strong class="fs-4">Vita: {{ pcCard.life }}</strong>
              </li>
            </ul>
          </div>
        </div>

        <div v-else>
          <p>nessuna carta scelta</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
  @import 'src/scss/general.scss';
</style>