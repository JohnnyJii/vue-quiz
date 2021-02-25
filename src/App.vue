<template>
  <div id="app">
    <Header class="header"
      :resetGame="resetGame"
      :questionsCorrect="questionsCorrect"
      :currentQuestion="index"
      :questionsTotal="questions.length"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="8" offset="2">
          <StartScreen 
            v-if="questions.length === 0" 
            :loadQuestions="loadQuestions"
          />
          <QuestionBox class="quiz"
            v-if="questions.length && !isGameOver"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
          <ScoreScreen 
            v-if="isGameOver"
            :score="questionsCorrect * 10"
          />
        </b-col>
      </b-row>
    </b-container>
    <Footer class="footer"/>
  </div>
</template>

<script>
import StartScreen from './components/StartScreen';
import Header from './components/Header';
import QuestionBox from './components/QuestionBox';
import ScoreScreen from './components/ScoreScreen';
import Footer from './components/Footer';

export default {
  name: 'App',
  components: {
    StartScreen,
    Header,
    QuestionBox,
    ScoreScreen,
    Footer,
  },
  data() {
    return {
      questions: [],
      index: 0,
      questionsCorrect: 0,
      questionsAnswered: 0,
      isGameOver: false,
    }
  },
  methods: {
    loadQuestions(amount, category, difficulty) {
      fetch(`https://opentdb.com/api.php?amount=${amount}${category}${difficulty}`, {
        method: 'get',
      }).then((response) => {
        return response.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results
      });
    },
    resetGame() {
      this.questions = [];
      this.index = 0;
      this.questionsCorrect = 0;
      this.questionsAnswered = 0;
      this.isGameOver = false;
    },
    next() {
      if (this.index < this.questions.length - 1) {
        this.index++;
      } else {
        this.isGameOver = true;
      }
      
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.questionsCorrect++;
      }
      this.questionsAnswered++;
    }
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
  margin-top: 0px;
  height: 900px;;
  background-color:#0048BA;
  background-image: linear-gradient(#0048BA, #5946B2);
}

.bv-example-row {
  -webkit-box-shadow: 0px 0px 10px 10px #000;
  border-radius: 25px;
  background: url('img/quiz.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  height: 491px;
  width: auto;
  position:inherit;
  color: aliceblue;
  opacity: 0.9;
  z-index: 100;
}

.header {
  padding-bottom: 15px;
}

.quiz {
  color: black;
}

.footer {
  padding: 50px;
  position: fixed;
  bottom: 0%;
  height: 50px;
  width: 100%;
  text-align: center;
  z-index: 10;

}
</style>
