<script>
import axios from "axios";
import { store } from './store';

export default {
  data() {
    return {
      api: store.api,
      title: "Card VS Card!",
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
    <h1 class="text-center text-light">{{ title }}</h1>
    <p style="font-size: 1.4rem; line-height: 1.7rem;" class="text-center fw-bold text-light">Pesca le carte, premi su FIGHT! e scopri qual è la più forte!</p>

    <!-- CARD -->
    <div class="row g-3 d-flex justify-content-center align-items-center">
      <div class="col-12 col-lg-5 d-flex flex-column align-items-center">
        <div class="text-center">
          <button class="btn btn-success fw-bold my-4" @click="handleStart('player')">PESCA UNA CARTA!</button>
        </div>
        
        <div class="text-center mt-3">
          <h4 class="text-light">USER</h4>
        </div>
        
        <div v-if="userCard" class="card border-0 bg-transparent w-100 mb-3">
          <div class="card-header border-0 p-0">
            <h5 class="card-title text-center fs-1 text-light">{{ userCard.name }}</h5>
          </div>
          <div class="card-body text-center p-0">            
            <img :src="api.baseUrl + userCard.type.image" class="image pb-2" alt="">
            <ul class="list-group list-group-flush">
              <li class="list-group-item p-0 bg-secondary ">
                <strong class="fs-5">Attacco: {{ userCard.strength }}</strong>
              </li>
              <li class="list-group-item p-0 bg-body-secondary">
                <strong class="fs-5">Difesa: {{ userCard.defence }}</strong>
              </li>
              <li class="list-group-item p-0 bg-secondary">
                <strong class="fs-5">Velocità: {{ userCard.speed }}</strong>
              </li>
              <li class="list-group-item p-0 bg-body-secondary">
                <strong class="fs-5">Vita: {{ userCard.life }}</strong>
              </li>
            </ul>
          </div>
        </div>

        <div v-else>
          <div class="text-center">
            <p class="text-warning fw-bold">NESSUNA CARTA SCELTA</p>
          </div>

        </div>
      </div>

      <div class="col-12 col-lg-2">
        <div class="text-center my-4">
          <button @click="fight()" class="text-center btn btn-danger fw-bold" id="ms-first-fight">FIGHT!</button>
        </div>
      </div>

      <div class="col-12 col-lg-5 d-flex flex-column align-items-center">
        <div class="text-center">
          <button class="btn btn-warning fw-bold my-4" @click="handleStart('pc')">PESCA LA CARTA DEL PC!</button>
        </div>
        
        <div class="text-center mt-3">
          <h4 class="text-light">COMPUTER</h4>
        </div>

        <div v-if="pcCard" class="card border-0 bg-transparent w-100 mb-3">
          <div class="card-header border-0 p-0 text-light">
            <h5 class="card-title text-center fs-1">{{ pcCard.name }}</h5>
          </div>
          <div class="card-body text-center p-0">
              <img :src="api.baseUrl + pcCard.type.image" class="image pb-2" alt="">
            <ul class="list-group list-group-flush">
              <li class="list-group-item p-0 bg-secondary">
                <strong class="fs-5">Attacco: {{ pcCard.strength }}</strong>
              </li>
              <li class="list-group-item p-0 bg-body-secondary">
                <strong class="fs-5">Difesa: {{ pcCard.defence }}</strong>
              </li>
              <li class="list-group-item p-0 bg-secondary">
                <strong class="fs-5">Velocità: {{ pcCard.speed }}</strong>
              </li>
              <li class="list-group-item p-0 bg-body-secondary">
                <strong class="fs-5">Vita: {{ pcCard.life }}</strong>
              </li>
            </ul>
          </div>
        </div>

        <div v-else>
          <div class="text-center">
            <p class="text-warning fw-bold">NESSUNA CARTA SCELTA</p>
          </div>
        </div>
      </div>
    </div>

    <div class="row">
      <div class="col-12 text-center mt-4 d-none" id="ms-second-fight">
        <button @click="fight()" class="text-center btn btn-danger fw-bold">FIGHT!</button>
      </div>
    </div>

  </div>
</template>

<style lang="scss">
  @import 'src/scss/general.scss';
</style>