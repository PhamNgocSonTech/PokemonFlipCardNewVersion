<template>
  <div class="screen">
    <h1>Interact Components In Here.....</h1>
    <CardFlipComponent
      v-for="(valueCard, index) in cardsContext"
      :key="index"
      :ref="`valueCard-${index}`"
      :backImageUrl="`img/${valueCard}.png`"
      :card="{ index, value: valueCard }"
      @onFlip="checkRule($event)"
    />
  </div>
</template>
<script>
import CardFlip from "./Card.vue";
export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlipComponent: CardFlip,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("Correct...");
        //add class: disabled to component card
        this.$refs[`valueCard-${this.rules[0].index}`][0].onEnableMode();
        this.$refs[`valueCard-${this.rules[1].index}`][0].onEnableMode();

        // reset rules to [] (empty)
        this.rules = [];
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        console.log("Wrong...");
        setTimeout(() => {
          // close two card
          this.$refs[`valueCard-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`valueCard-${this.rules[1].index}`][0].onFlipBackCard();
          //reset rule to []
          this.rules = [];
        }, 650);
      } else {
        return false;
      }
    },
  },
};
</script>
