<script setup>
import {ref, computed} from 'vue'

const questions = ref([
  {
    question: 'what is vue js?',
    answer:0,
    options: [
      'a fron end framework',
      'a library',
      'an ice-cream maker'
    ],
    selected: null
  },
  {
    question: 'what is vuex?',
    answer:2,
    options: [
           'vue with an x',
           'a cheese selection',
           'a state management library'
    ],
    selected: null
  },
  {
    question: 'what is vue Router used for?',
    answer:1,
    options: [
           'walking in space',
           'a routing library for vue js',
           'burger sauce'
    ],
    selected: null
  }
])

const quizCompleted=ref(false)
const currentQuestion= ref(0)
const score = computed(()=>{
  let value=0
  questions.value.map(q => {
    if(q.selected == q.answer){
         value++
    }
  })
  return value
})

const getCurrentQuestion = computed(()=>{
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})
const SetAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value
  evt.target.value = null
}

const NextQuestion = () => {
  if(currentQuestion.value < questions.value.length-1) {
    currentQuestion.value++
  } else{
     quizCompleted.value = true
  }
}
</script>

<template>
  <main>
    <h1>The Quizz</h1>

    <section v-if="!quizCompleted">
      <div>
        <span>{{ getCurrentQuestion.question }}</span>
        <span>score {{ score }}/{{ questions.length }}</span>
        </div>

        <div>
          <label v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected == index 
            ? index == getCurrentQuestion.answer
            ? 'correct'
            : 'wrong'   
            : ''
          }${
            getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
            ? 'disabled'
            : ''
          }`">
          
            <input type="radio"
             :name="getCurrentQuestion.index" 
             :value="index"
             v-model="getCurrentQuestion.selected"
             :disabled="getCurrentQuestion.selected"
             @change="SetAnswer"
             />
             <span>{{ option }}</span>
            </label>
          </div>
          <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
            {{ getCurrentQuestion.index == questions.length -1
            ? 'Finish'
            : getCurrentQuestion.selected == null
              ? 'select an option'
              : 'Next Question'
           }}
            </button>
      </section>
      <section v-else>
        <h2>You have finished the quiz!</h2>
        <p>your score is {{ score }}/{{ questions.length }}</p>
        </section>
    </main>
</template>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

body{
  background-color: black;
  color: #fff;
}
.app{
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 3rem;
  min-height: 100vh;
}
h1{
  font-size: 4rem;
  margin-bottom: 2rem;

}
.quiz{
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;

}
.quiz-info {
  display: flex;
  justify-content: center;
  margin-bottom: 1rem;
}
.quiz-info .question{
  color: black;
  font-size: 3rem;
}
.quiz-info .score {
  color: #fff;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 1rem;
}

.option {
  display: flex;
  padding: 1rem;
  border-radius: 0.5rem;
  margin-bottom: 0.5rem;
  background-color: #271C36;
  cursor: pointer;

}

.option:hover {
  background-color: #382a4b;
}
.option.correct {
  background-color: aqua;
}

.option.wrong{
  background-color: red;
}








</style>
