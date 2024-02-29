<template>
  <MainScreen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <InteractScreen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <ResultScreen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onTryAgain="statusMatch = 'default'"
  />
  <CopyRightScreen />
</template>

<script>
import MainScreen from "./components/MainScreen";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRightScreen from "./components/CopyRightScreen.vue";
import { shuffled } from "./utils/array";
export default {
  name: "App",
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
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRightScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      this.settings.startedAt = new Date().getTime();
      this.statusMatch = "match";
    },
    onGetResult() {
      //get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      //switch to result component
      this.statusMatch = "result";
    },
  },
};
</script>
