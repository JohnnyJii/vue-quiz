<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectAnswer(index)"
          :class="[answerClass(index)]"
          :disabled="answered === true"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="success" @click="next" :disabled="answered === false">
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';
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
      answered: false,
    };
  },
  computed: {
    answers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      answers = _.shuffle(answers);
      return answers;
    },
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
      },
    },
  },
  methods: {
    answerClass(index) {
      let answerClass = '';
      if (this.answered && this.correctIndex === index) {
        answerClass = 'correct';
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correct_answer !== index
      ) {
        answerClass = 'wrong';
      }
      return answerClass;
    },
    selectAnswer(index) {
      this.selectedIndex = index;
      this.answered = true;
      this.checkAnswer();
    },
    checkAnswer() {
      this.correctIndex = this.answers.indexOf(
        this.currentQuestion.correct_answer
      );
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }

      this.increment(isCorrect);
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}

.btn {
  margin: 5px;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: green;
  color: white;
}

.wrong {
  background-color: red;
  color: white;
}
</style>
