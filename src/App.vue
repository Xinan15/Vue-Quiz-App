<script setup>
// in <script setup>, you should write your component's logic.
// Such as reactive state, computed properties, and methods.
// Vue's Composition API

import { ref, computed } from 'vue'

// ref denotes reference
// ref() is a function in Vue.js that creates a reactive reference. 
// a reactive reference in Vue is just the same as a state variable in React.

const questions = ref([
  {
    question: 'What is Vue.js?',
    answer: 2,
    // 2 here is the index of the correct answer
    options: [
      'A back-end server framework.',
      'A database management system.',
      'A JavaScript framework for building user interfaces.'
    ],
    selected: null
    // Here means initially no option is selected
  },
  {
    question: 'Is Vue.js a front-end or back-end framework?',
    answer: 0,
    options: [
      'Front-end',
      'Back-end',
      'Neither',
    ],
    selected: null
  },
  {
    question: ' What language is Vue.js written in?',
    answer: 1,
    options: [
      'Python',
      'JavaScript',
      'Java',
    ],
    selected: null
  }
]);

// here means initially quiz is not completed
const quizCompleted = ref(false);

// here means initially the index of the current question is 0
const currentQuestion = ref(0);

// The computed() function in Vue.js is used to create a computed property. 
// A computed property is a property whose value depends on one or more other properties, called dependencies.
// Whenever a dependency changes, the computed property will be re-evaluated.
const score = computed(() => {
  let score = 0;
  questions.value.forEach(question => {
    if (question.answer === question.selected) {
      score++;
    }
  });
  return score;
});

// The forEach() method in JavaScript executes a provided function once for each array element in an array.
// question => arrow function: the function takes 'question' as a parameter.
// The function body comes after the =>, in the curly braces.
// questions is a ref() defined above, which is an array of objects.
// so, questions.value, is accessing the current value of the questions reactive reference.

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});

// evt denotes the event object
// Here, you are defining computed properties for the component. 

const SetAnswer = evt => {
  if (questions.value[currentQuestion.value].selected === null) {
    questions.value[currentQuestion.value].selected = parseInt(evt.target.value);
  }
};
// [currentQuestion.value] is the index of the current question
// questions.value[currentQuestion.value]: is accessing the specific question object

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
  } else {
    quizCompleted.value = true;
  }
};
</script>

<template>
  <!-- <template> contains the HTML that will be rendered for the component. -->
  <main class="app">
    <h1>The Quiz</h1>
    <!-- The v-if directive is used in Vue.js to conditionally render an element only if the directive's expression returns a truthy value. -->
    <!-- <section class="quiz" v-if="!quizCompleted"> will render the section element with the class quiz only if quizCompleted is false -->

    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }}/{{ questions.length }}</span>
      </div>

      <!-- The v-for directive renders a list of items based on an array.  -->
      <div class="options">
        <label v-for="(option, index) in getCurrentQuestion.options" :for="'option' + index" :class="`option ${getCurrentQuestion.selected == index
          ? index == getCurrentQuestion.answer
            ? 'correct'
            : 'wrong'
          : ''
          } ${getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
            ? 'disabled'
            : ''
          }`">
          
          <input type="radio" :id="'option' + index" :name="getCurrentQuestion.index" :value="index"
            :checked="getCurrentQuestion.selected === index" @input="SetAnswer" />

          <span>{{ option }}</span>
        </label>
      </div>
<!-- The @click and @input are event listeners. They call a method when an event occurs. -->
      <button @click="NextQuestion" :disabled="getCurrentQuestion.selected === null">
        {{
          getCurrentQuestion.index == questions.length - 1
          ? 'Finish'
          : getCurrentQuestion.selected == null
            ? 'Select an option'
            : 'Next question'
        }}
      </button>
    </section>

    <section v-else>
      <h2>You have finished the quiz!</h2>
      <p>Your score is {{ score }}/{{ questions.length }}</p>
    </section>
  </main>
</template>

<style scoped>
/* 'scoped' attribute here limits the CSS within that tag to the current component only.
This means the styles defined here will not leak out to child or parent components. */
* {
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

body {
  background-color: #D7BBF5;
  color: #FFF;
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  height: 100vh;
  margin-top:2rem;
}

h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.quiz {
  background-color: #A076F9;
  padding: 4rem;
  width: 100%;
  max-width: auto;
  border-radius: 1rem;  
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
  align-items: center;
}

.quiz-info .question {
  color: #FFF;
  font-size: 1.25rem;
  font-weight: bold;
  padding:10px;
}

.quiz-info.score {
  color: #FFF;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 2rem;
}

.option {
  padding: 1rem;
  margin: 1rem;
  display: block;
  background-color: #6528F7;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover {
  background-color: #2d213f;
}

.option.correct {
  background-color: #2cce7d;
}

.option.wrong {
  background-color: #ff5a5f;
}

.option:last-of-type {
  margin-bottom: 0;
}

.option.disabled {
  opacity: 0.5;
}

.option input {
  display: none;
}

button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.2rem;
  border-radius: 0.5rem;
}

button:disabled {
  opacity: 0.5;
}

h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}

p {
  color: #A076F9;
  font-size: 1.5rem;
  text-align: center;
  font-weight: bold;
}
</style>
