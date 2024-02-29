<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="onStartAgain"
  />
  <copy-right />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffled } from "./utils/array.js";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRight.vue";
// function randomImages(min, max) {
//   return Math.floor(Math.random() * (max - min + 1)) + min;
// }
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
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
      console.log("running handle before start", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const allNumbers = Array.from({ length: 64 }, (_, i) => i + 1);

      // Trộn lẫn mảng các số
      const shuffledNumbers = shuffled(allNumbers);

      // Lấy N/2 phần tử đầu tiên của mảng đã trộn để tạo danh sách số không trùng nhau
      const uniqueNumbers = shuffledNumbers.slice(
        0,
        this.settings.totalOfBlocks / 2
      );

      // Tạo mảng chứa các số ngẫu nhiên từ danh sách uniqueNumbers
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => uniqueNumbers[i]
      );

      const secondCards = [...firstCards];
      console.log(firstCards);
      console.log(secondCards);
      const cards = [...firstCards, ...secondCards];
      console.log(cards);
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      this.settings.startedAt = new Date().getTime();
      console.log(this.settings.cardsContext);
      this.statusMatch = "match";
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt;
      this.statusMatch = "result";
    },
    onStartAgain() {
      this.statusMatch = "default"; // Đặt lại trạng thái về "default"
    },
  },
};
</script>

<style></style>
