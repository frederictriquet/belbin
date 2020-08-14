<template>
  <div id="app">
    <md-toolbar class="md-primary">
        <h1 class="md-title">Questionnaire Belbin</h1>
    </md-toolbar>
    <md-content>
      <quiz-belbin-question v-for="(question,index) in questions" :key="index"
        :title="question['title']"
        :items="question['answers']"
        />
    </md-content>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import QuizBelbinQuestion from './components/QuizBelbinQuestion.vue'
import axios from 'axios'

export default Vue.extend({
    name: 'App',
    components: {
        QuizBelbinQuestion
    },
    data: () => ({
        questions: null
    }),
    created () {
      axios
        .get('http://localhost:8080/data.json')
        .then(response => (this.questions = response.data))
    }
})
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
</style>
