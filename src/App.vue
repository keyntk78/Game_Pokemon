<template>
  <main-screen
    v-if="startusMatch === 'default'"
    @onStart="($event) => onHandleBeforStart($event)"
  />
  <interct-screen
    v-if="startusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="() => onGetResult()"
  />
  <ResultScreen
    :timer="timer"
    v-if="startusMatch === 'result'"
    @onstartAgain="() => (startusMatch = 'default')"
  />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InterctScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";

import { shuffled } from "./utils/array";

export default {
  name: "App",
  data() {
    return {
      settings: {
        totalBlock: 0,
        startedAt: null,
        cardsContext: [],
      },
      timer: null,
      startusMatch: "default",
    };
  },
  components: {
    MainScreen,
    InterctScreen,
    ResultScreen,
  },
  methods: {
    onHandleBeforStart(config) {
      console.log("runnng onHandleBeforStart: ", config);
      this.settings.totalBlock = config.totalBlock;
      const fisrtCards = Array.from(
        { length: this.settings.totalBlock / 2 },
        (_, i) => i + 1
      );

      const secondCards = [...fisrtCards];
      const card = [...fisrtCards, ...secondCards];

      this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(card))));
      this.settings.startedAt = new Date().getTime();

      this.startusMatch = "match";
    },
    onGetResult() {
      // get timer
      this.timer = new Date().getTime() - this.settings.startedAt;

      this.startusMatch = "result";
    },
  },
};
</script>
