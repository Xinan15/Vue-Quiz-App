<script setup>
// <script setup> is to create a component without the need to export it.
// ref is used to create reactive data, and computed is used to create computed properties.

import { ref, computed } from 'vue'

// Here we create a reactive variable called questions
// ref denotes reference
const questions = ref([
  {
    question: 'What is Vue.js?',
    answer: 2,
    // O here is the index of the correct answer
    options: [
      'A back-end server framework.',
      'A database management system.',
      'A JavaScript framework for building user interfaces.'
    ],
    selected: null
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

const quizCompleted = ref(false);

const currentQuestion = ref(0);

// These lines are declaring the reactive data for your component.
// Any changes to these variables will cause the component to re-render.

const score = computed(() => {
  let score = 0;
  questions.value.forEach(question => {
    if (question.answer === question.selected) {
      score++;
    }
  });
  return score;
});

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});

// evt denotes the event object
// Here, you are defining computed properties for the component. 
// These properties will automatically update whenever any of the reactive data they depend on changes.
const SetAnswer = evt => {
  questions.value[currentQuestion.value].selected = parseInt(evt.target.value);
};


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

    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }}/{{ questions.length }}</span>
      </div>

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

<!-- 
  <template> contains the HTML that will be rendered for the component.

The v-if directive is used to conditionally render elements based on the value of quizCompleted. If the quiz is not completed, it shows the quiz; otherwise, it shows the result.
The v-for directive is used to render a list of elements based on an array.
The :for, :class, :id, :name, :value, :checked, and :disabled are all examples of bindings. They dynamically bind DOM attributes to data.
The @click and @input are event listeners. They call a method when an event occurs.
 -->
<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

body {
  background-color: #2d213f;
  color: #FFF;
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  height: 100vh;
}

h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.quiz {
  background-color: #382a4b;
  padding: 5rem;
  width: 100%;
  max-width: 640px;

}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: #FFF;
  font-size: 1.25rem;
}

.quiz-info.score {
  color: #FFF;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 1rem;
}

.option {
  padding: 1rem;
  display: block;
  background-color: #271c36;
  margin-bottom: 0.5rem;
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
  color: #8F8F8F;
  font-size: 1.5rem;
  text-align: center;
}
</style>
