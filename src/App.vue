<template>
  <v-app class="app">
    <v-app-bar
      app
      color="primary"
      dark
    >
      <v-icon name="brands/vuejs" color="white" scale="3" />
    </v-app-bar>
    <v-content class="gameboardSection">
      <Gameboard  @updateGameStatus="updateGameStatus"/>
    </v-content>
    <v-footer
      class="footer"
      color="blue"
      height="60"
      fixed
     >
     <div>Question #{{question}} of {{totalQuestions}}</div>
     <div>Your current score: {{currentScore}}</div>
    </v-footer>
  </v-app>
</template>

<script>
import Gameboard from './components/Gameboard';
import Icon from 'vue-awesome/components/Icon'

export default {
  name: 'App',
  components: {
    Gameboard,
    'v-icon': Icon
  },
  data() {
    return {
      answer: false,
      question: 1,
      currentScore: 0,
      totalQuestions: 20,
    }
  },
  methods: {
      updateGameStatus(payload) {
        this.currentScore = payload.currentScore
        this.question = payload.question
        this.answer = payload.answer
      }
  },
};
</script>

<style>
.gameboardSection {
  display: flex;
  justify-content: center;
  align-items: center;
}
.footer {
  display: flex;
  justify-content: space-between;
}
</style>
