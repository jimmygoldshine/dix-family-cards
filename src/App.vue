<template>
  <div class="container" id="app">
      <div class="form">
        <input type='text' placeholder="Player name" v-model='newPlayer.name'>
        <input type='submit' value='Submit' v-on:click='addPlayer'>
      </div>
      <player-group :players="players" v-on:selectPlayer="selectPlayer" />
      <player-group :players="selectedPlayers" />
      <button v-on:click='assignHeats'>Assign Heats</button>
      <div class="heat" v-for="(heat, index) in heats">
        <h2>Heat: {{ index+1 }}</h2>
        <player-group :players="heats[index]" v-on:selectPlayer="finalist" />
      </div>
      <div class="container" id="final">
        <h2>Final</h2>
        <player-group :players="final" />
      </div>
    </div>
</template>

<script>

import Firebase from 'firebase'
import PlayerGroup from './components/player-group';
let players = require('./dix-players');

let config = {
    apiKey: "AIzaSyDGUyHQU8QYd07fTZch23egxUK6f3qKlpg",
    authDomain: "dix-family-cards.firebaseapp.com",
    databaseURL: "https://dix-family-cards.firebaseio.com",
    projectId: "dix-family-cards",
    storageBucket: "dix-family-cards.appspot.com",
    messagingSenderId: "1014029156759"
};

let app = Firebase.initializeApp(config)
let db = app.database()
let playersRef = db.ref('players')

export default {
  name: 'app',

  components: {
    PlayerGroup,
  },

  firebase: {
    players: playersRef
  },

  data() {
    return {
      newPlayer: {
        name: ''
      },
      selectedPlayers: [],
      heats: [],
      final: []
    }
  },

  methods: {
    addPlayer: function() {
      playersRef.push(this.newPlayer);
      // this.playerPool.push({'name': this.player});
      this.newPlayer.name = '';
    },

    selectPlayer: function(player) {
      this.selectedPlayers.push(player);
      this.removePlayer(player)
    },

    removePlayer: function(player) {
      const index = this.playerPool.indexOf(player);
      this.playerPool.splice(index, 1);
    },

    finalist: function(player) {
      this.final.push(player)
    },

    assignHeats: function() {
      const players = this.assignIds();
      const sortedPlayers = this.sortById(players);
      while(sortedPlayers.length > 0) {
        let playersInHeat = sortedPlayers.splice(0, 4);
        this.heats.push(playersInHeat);
      }
    },

    sortById: function(players) {
      return players.sort((a,b) => {
        return a.id - b.id
      })
    },

    assignIds: function() {
      return this.selectedPlayers.map((player) => {
        player['id'] = Math.random();
        return player
      });
    }
  }

}
</script>

<style>

#app {
}

</style>
