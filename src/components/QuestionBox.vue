<template>
  <div>
    <b-jumbotron class="text-center py-4">
      <template>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group class="mb-4">
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        class="my-0 mx-2"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
        >Submit</b-button
      >
      <b-button variant="success" class="my-0 mx-2" @click="next"
        >Next</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    next: Function,
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
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let array = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];

      const length = array == null ? 0 : array.length;
      if (!length) {
        return [];
      }
      let index = -1;
      const lastIndex = length - 1;
      const result = [...array];
      while (++index < length) {
        const rand =
          index + Math.floor(Math.random() * (lastIndex - index + 1));
        const value = result[rand];
        result[rand] = result[index];
        result[index] = value;
      }

      return (
        (this.shuffledAnswers = result),
        (this.correctIndex = this.shuffledAnswers.indexOf(
          this.currentQuestion.correct_answer,
        ))
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = '';
      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'bg-info text-white';
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'bg-success text-white';
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = 'bg-danger text-white';
      }

      return answerClass;
    },
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
  computed: {
    answers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      return answers;
    },
  },
};
</script>

<style scoped>
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}

.selected {
  background-color: #007bff;
}

.correct {
  background-color: #28a745;
}

.incorrect {
  background-color: #dc3545;
}
</style>
