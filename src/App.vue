<template>
  <div id="app">
    <Header :numTotal="numTotal" :numCorrect="numCorrect" :numQuestion="index" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col md="6" offset-md="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :back="back"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    next() {
      if (this.index < 9) {
        this.index++;
      } else return;
    },
    back() {
      if (this.index > 0) {
        this.index--;
      } else return;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
  },
  mounted: function () {
    fetch("https://opentdb.com/api.php?amount=10&encode=url3986", {
      method: "get",
    })
      .then((response) => response.json())
      .then((jsonData) => (this.questions = jsonData.results));
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

@media screen and (max-width: 576px) {
}
</style>
