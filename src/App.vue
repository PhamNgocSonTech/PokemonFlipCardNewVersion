<template>
  <MainScreenComponent
    v-if="statusMatch === 'default'"
    v-on:onStart="onHandleBeforeStart($event)"
  />

  <InteractScreenComponent
    v-if="statusMatch === 'match'"
    v-bind:cardsContext="settings.cardsContext"
    v-on:onFinish="onGetResult"
  />

  <ResultScreenComponent
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />

  <CopyrightComponent />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import Copyright from "./components/FooterScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { suffled } from "./utils/array";
export default {
  name: "App",
  components: {
    MainScreenComponent: MainScreen,
    CopyrightComponent: Copyright,
    InteractScreenComponent: InteractScreen,
    ResultScreenComponent: ResultScreen,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
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
    onGetResult() {
      // get timer
      this.timer = new Date().getTime() - this.settings.startedAt;

      // get result to component
      this.statusMatch = "result";
    },
  },
};
</script>

<style>

</style>
