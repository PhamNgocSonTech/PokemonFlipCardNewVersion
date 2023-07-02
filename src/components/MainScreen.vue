<template>
  <div class="actions">
    <h1>POKE FLIP CARD GAME</h1>
    <h3>Select Mode To Start Game</h3>
  </div>
  <button v-on:click="onStart(16)">
    <span>4x4</span>
    <span>Easy</span>
  </button>

  <button v-on:click="onStart(36)">
    <span>6x6</span>
    <span>Normal</span>
  </button>

  <button v-on:click="onStart(64)">
    <span>8x8</span>
    <span>Hard</span>
  </button>

  <button v-on:click="onStart(100)">
    <span>10x10</span>
    <span>Very Hard</span>
  </button>
</template>

<script>
export default {
  methods: {
    onStart(totalOfBlocks) {
      this.$emit('onStart', { totalOfBlocks })
    }
  }
}
</script>

<style></style>
