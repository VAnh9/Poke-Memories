<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"/>
  <interact-screen 
    v-if="statusMatch === 'match'" 
    :cardsContext="setting.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen :timer="timer" v-if="statusMatch === 'result'" @onStart="onStartAgain" />
  <copy-right-screen />
</template>

<script>
import MainScreen from "./components/MainScreen.vue"
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRightScreen from "./components/CopyRightScreen.vue";
import { shuffled } from "./utils/array.js"
export default {
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRightScreen
  },
  data() {
    return {
      setting: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    }
  },
  methods: {
    onHandleBeforeStart(config) {
      this.setting.totalOfBlocks = config.totalOfBlocks
      const firstCards = Array.from( {length: this.setting.totalOfBlocks / 2}, (_, i) => i + 1)
      const secondeCards = [...firstCards]
      const cards = [...firstCards, ...secondeCards]
      this.setting.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))))

      this.setting.startedAt = new Date().getTime()
      // data ready
      this.statusMatch = "match"
    },
    onGetResult() {
      //get timer
      this.timer = new Date().getTime() - this.setting.startedAt
      //switch to result component
      this.statusMatch = "result"
    },
    onStartAgain() {
      this.statusMatch = 'default'
    }
  }
}
</script>

<style>

</style>
