<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{ currentQuestion.question }}</template>
      <hr class="my-4" />
      <b-list-group v-for="(answer, index) in answers" :key="index">
        <b-list-group-item
          @click.prevent="selectAnswer(index); getCorrectIndex(answers)"
          :class="answerClass(index)"
        >{{ answer }}</b-list-group-item>
      </b-list-group>
      <b-button 
      variant="primary"
      @click="submitAnswer"
      :disabled="selectedIndex === null || answered">submit</b-button>
      <b-button variant="success" @click="next">next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data(){
    return {
      shuffledAnswers: [],
      selectedIndex: null,
      correctIndex: null,
      answered:false
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      answers.sort(() => 0.5 - Math.random())
      return answers;
    },
  },
  watch:{
    currentQuestion:{
      immediate: true,
      handler(){
        this.answered = false
        this.selectedIndex = false
        console.log(this.answered)
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
    },
    getCorrectIndex(answers){
      this.correctIndex = answers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer(){
      let isCorrect = false

      if(this.selectedIndex === this.correctIndex){
        isCorrect = true
      }

    this.answered = true
    this.increment(isCorrect)

    },
    answerClass(index) {

      let answerClass = '';

      if(!this.answered && this.selectedIndex === index){
        answerClass = 'selected'
      }

      if(this.answered && this.correctIndex === index){
        answerClass = 'correct'
      }

      if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
        answerClass = 'incorrect'
      }
          
      return answerClass;
    }
  }
};

</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.btn {
  margin: 0 5px;
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
  background-color: lightcoral;
}
</style>