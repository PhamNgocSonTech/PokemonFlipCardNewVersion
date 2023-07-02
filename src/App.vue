<template>
  <MainScreenComponent
    v-if="statusMatch === 'default'"
    v-on:onStart="onHandleBeforeStart($event)"
  />

  <InteractScreenComponent
    v-if="statusMatch === 'match'"
    v-bind:cardsContext="settings.cardsContext"
  />

  <FooterComponent />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import FooterCopyRightScreen from "./components/FooterScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { suffled } from "./utils/array";
export default {
  name: "App",
  components: {
    MainScreenComponent: MainScreen,
    FooterComponent: FooterCopyRightScreen,
    InteractScreenComponent: InteractScreen,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("Running Handle", config);
      //data ready
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      this.statusMatch = "match";
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];

      this.settings.cardsContext = suffled(suffled(suffled(suffled(cards))));

      this.settings.startedAt = new Date().getTime();
    },
  },
};
</script>

<style></style>
