<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <CardFlipComponent
        v-for="(valueCard, index) in cardsContext"
        :key="index"
        :ref="`valueCard-${index}`"
        :cardsContext="cardsContext"
        :backImageUrl="`img/${valueCard}.png`"
        :card="{ index, value: valueCard }"
        :rules ="rules"
        @onFlip="checkRule($event)"
      />
    </div>
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

        const disableElements = document.querySelectorAll(
          ".screen .card.disabled"
        );
        // if disableElements == total elements - 2 => all card is flipped
        if (
          disableElements &&
          disableElements.length === this.cardsContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
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

<style scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  width: calc(424px);
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}

</style>
