<template>
  <div>
    <div class="player-group" id="player-pool">
      <button v-for='(player, index) in players' v-on:click='selectPlayer(player)'>{{ player.name }}</button>
    </div>
  </div>
</template>

<script>

export default {

  name: 'PlayerPool',

  props: ['players'],

  methods: {
    selectPlayer: function(player) {
      this.$emit('selectPlayer', player)
    }
  }

}

</script>

<style scoped>

</style>
