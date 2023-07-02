<template>
  <div class="screen">
    <h1>Interact Components In Here.....</h1>
    <card-flip
      v-for="(card, index) in cardsContext"
      :key="index"
      :imgBackFaceUrl="`img/${card}.png`"
    />
  </div>
</template>
<script>
import CardFlip from './Card.vue'
export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return []
      }
    }
  },
  components: {
    CardFlip
  }
}
</script>
