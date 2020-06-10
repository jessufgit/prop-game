<template>
  <div class="wrapper">
    <div class="game" v-if="this.question <= problemsArray.length">
      <div class="gameBoard"
          v-for="problem in problemsArray"
          :key="problem.id"
      >
        <div
          class="gamecard"
          v-if="problem.activeQuestion">
          <v-card
            color="white"
            elevation="10"
            min-height="300"
            min-width="500">
            <v-card-title>Question:</v-card-title>
            <v-card-text>{{problem.question}}</v-card-text>
            <v-card-title>Answer:</v-card-title>
            <v-card-text>
              <div v-if="complete">The correct answer is {{problem.answer}}. You answered {{ answer }}.
              </div>
            </v-card-text>
            <v-card-actions class="stateOfTheGame">
              <v-btn
                v-for="button in buttonChoices" :key="button.id"
                value='button.name'
                @click="emitChoice(button.name, problem)"
                :name="button.name"
                class="answerButton">{{button.name}}
              </v-btn>
            </v-card-actions>
          </v-card>
        </div>
      </div>
    </div>
    <div class="game" v-else>
      <div class="gameOverMsg">
        <h1>Game Over</h1>
        <v-btn @click="resetGame">Play Again</v-btn>
      </div>
    </div>
  </div>
</template>

<script>
import problems from '../problems.json'

export default {
  data() {
    return {
      problemsArray: problems,
      complete: false,
      currentScore: null,
      question: null,
      answer: null,
      wrong: false,
      buttonChoices: [
        {'id': 1, 'name': true},
        {'id': 2, 'name': false}
      ],
    }
  },
  methods: {
    checkAnswer(problem) {
      if (this.answer === problem.answer) {
        this.correctAnswer();
      } else {
        this.wrongAnswer();
      }
    },
    correctAnswer() {
      setTimeout(() => this.complete = true, 800);
      this.currentScore+=5
    },
    wrongAnswer() {
      setTimeout(() => this.complete = true, 800);
      this.currentScore -= 5
    },
    changeQuestions(problem) {
      this.complete = false
      problem.activeQuestion = false
      this.question++
      if (this.question <= this.problemsArray.length) {
        this.problemsArray[problem.id].activeQuestion = true
      } else {
        console.log("Game Over")
      }
    },
    emitChoice(buttonName, problem) {
      this.answer = buttonName
      this.complete = true
      this.checkAnswer(problem)
      setTimeout(() => this.changeQuestions(problem), 2000);
      const payload = {
        currentScore: this.currentScore,
        question: this.question,
        answer: buttonName,
      };
      this.$emit('updateGameStatus', payload)
    },
  },
  created: function() {
      this.currentScore = 0
      this.question = 1
  },
  updated() {

  }
}
</script>

<style>
.game {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: -60px;
}
.stateOfTheGame {
  display: flex;
  justify-content: space-evenly;
}
.answerButton {
  margin: 5px;
}
.gameOverMsg {
  text-align: center;
  display: flex;
  flex-flow: wrap column;
  justify-content: center;
  align-content: center;
}
h1 {
  margin-bottom: 20px;
}
</style>
