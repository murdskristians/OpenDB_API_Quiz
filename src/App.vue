<template>
  <div id="app">
    <Header 
    :totalAnswers='totalAnswers'
    :correctAnswers='correctAnswers'
    />

<b-container class="bv-example-row">
  <b-row>
    <b-col sm=10 offset="1" >
      <QuestionBox 
      v-if="questions.length"
      :currentQuestion="questions[index]"
      :next="next"
      :increment="increment"
      />
    </b-col>
  </b-row>
</b-container>

    
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions : [],
      index: 0,
      totalAnswers: 0,
      correctAnswers: 0
    }
  },
  methods: {
    next() {
      if (this.index < 9) {
        this.index++;
      }
      else {
        alert('You have finished all 10 questions. Refresh page to restart the quiz.')
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.correctAnswers++
      }
      this.totalAnswers++
    }
  },
  mounted: function() {
    //Gets questions and answers from API database
    fetch("https://opentdb.com/api.php?amount=10&category=18&type=multiple", { 
      method: 'get'
    })
      .then((response) => { 
        return response.json()
      })
      .then((dataJson) => { 
        this.questions = dataJson.results
      })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}
</style>
