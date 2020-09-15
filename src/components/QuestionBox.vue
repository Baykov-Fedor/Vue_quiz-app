<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{ decodeURIComponent(currentQuestion.question) }}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
        >{{ decodeURIComponent(answer) }}</b-list-group-item>
      </b-list-group>
      <div class="button-container">
        <b-button variant="primary" @click="back">&#x3C; Previous</b-button>
        <b-button
          variant="success"
          @click="submitAnswer"
          :disabled="selectedIndex === null || answered"
        >Submit</b-button>
        <b-button variant="primary" @click="next">Next &#x3E;</b-button>
      </div>
    </b-jumbotron>
  </div>
</template>

<script>
import shuffle from "./shuffle.utils";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    back: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    answerClass(index) {
      let answerClass = "";
      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = "incorrect";
      }
      return answerClass;
    },
  },
};
</script>

<style>
.button-container {
  margin-top: 20px;
  display: flex;
  justify-content: space-around;
}

.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: lightsalmon;
}
</style>