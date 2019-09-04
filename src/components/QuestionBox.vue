<template>
  <div>
    <b-jumbotron>
      <template slot="lead">
        {{ HtmlEncode(currentQuestion.question) }}
      </template>

      <hr class="my-4">


      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers" 
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{ HtmlEncode(answer) }}
        </b-list-group-item>
      </b-list-group>


      <b-button variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === NULL || isAnswered"
      >
        Submit
      </b-button>

      <b-button 
        @click="next" 
        variant="success" 
        href="#"
      >
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash"

export default {
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      isAnswered: false
    }
  },
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers(),
        this.isAnswered = false
      }
    }

//Will work the same as this, but will shuffle when first question arrives.
    // (){
    //   this.selectedIndex = null;
    //   this.shuffleAnswers()
    // }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)

      return answers
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswer() {
      let isCorrect = false

      if ( this.selectedIndex === this.correctIndex )
      {
        isCorrect = true;
      }
      this.isAnswered = true
      this.increment(isCorrect);
    },
    shuffleAnswers() {
      let answers = 
      [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]

      this.shuffledAnswers = _.shuffle(answers)

      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    answerClass(index) {
      let className = ''

      if (!this.isAnswered && this.selectedIndex === index){
        className = 'selected'
      } else if (this.isAnswered && index != this.correctIndex && index === this.selectedIndex) {
        className = 'incorrect'
      } else if (this.isAnswered && index === this.correctIndex) {
        className = 'correct'
      }

      return className
    },
    HtmlEncode(text) {
      let dedcodedText = document.createElement("div");
      dedcodedText.innerHTML = text;
      return dedcodedText.innerText;
    }
  }
}
</script>

<style scoped>
.list-group {
  margin-bottom: 10px;
}
.list-group-item {
  margin-top: 5px;
}
.list-group-item:hover {
  background: #EEE;
  cursor: pointer;
}
.btn {
  margin: 0 20px;
}
.selected {
  background-color: yellow;
}
.correct {
  background-color: rgb(108, 201, 108);
}
.incorrect {
  background-color: rgb(247, 151, 151);
}
</style>
