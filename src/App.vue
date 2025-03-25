<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
	question: 'What is Vue?',
	answer: 0,
	options: [
		'A framework',
		'A library',
		'A type of hat'
	],
	selected: null
  },
  {
	question: 'What is Vuex used for?',
	answer: 2,
	options: [
		'Eating a delicious snack',
		'Viewing things',
		'State management'
	],
	selected: null
  },
  {
	question: 'What is Vue Router?',
	answer: 1,
	options: [
		'An ice cream maker',
		'A routing library for Vue',
		'Burger sauce'
	],
	selected: null
  }
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected != null && q.answer == q.selected) {
			console.log('correct');
			value++
		}
	})
	return value
})

const getCurrentQuestion = computed(() => {
	let question = questions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})

const SetAnswer = (e) => {
	questions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}

const NextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}
	
	quizCompleted.value = true
}
</script>

<template>
	<main class="app">
		<h1>The Quiz</h1>
		
		<section class="quiz" v-if="!quizCompleted">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score">Score {{ score }}/{{ questions.length }}</span>
			</div>
			
			<div class="options">
				<label 
					v-for="(option, index) in getCurrentQuestion.options" 
					:for="'option' + index" 
					:class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer 
								? 'correct' 
								: 'wrong'
							: ''
					} ${
						getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
							? 'disabled'
							: ''
					}`">
					<input 
						type="radio" 
						:id="'option' + index" 
						:name="getCurrentQuestion.index" 
						:value="index" 
						v-model="getCurrentQuestion.selected" 
						:disabled="getCurrentQuestion.selected"
						@change="SetAnswer" 
					/>
					<span>{{ option }}</span>
				</label>
			</div>
			
			<button 
				@click="NextQuestion" 
				:disabled="!getCurrentQuestion.selected">
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

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

body {
  background-color: #f0f4f8; /* Light background for a fresh look */
  color: #333333; /* Dark text for readability */
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  overflow: hidden; /* Prevent unnecessary scrollbars */
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  width: 100%;
  max-width: 800px; /* Increased max-width for a larger container */
  border-radius: 1rem;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  background-color: #ffffff; /* White background for the quiz container */
}

h1 {
  font-size: 3rem;
  margin-bottom: 2rem;
  color: #ff6f61; /* Vibrant coral color for the title */
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  align-items: center; /* Align items vertically */
  width: 100%;
  margin-bottom: 2rem;
  gap: 1rem; /* Add gap between question and score */
}

.quiz-info .question {
  color: #ff6f61; /* Consistent accent color for questions */
  font-size: 1.75rem;
}

.quiz-info .score {
  color: #333333; /* Consistent text color */
  font-size: 1.75rem;
}

.options {
  width: 100%;
  margin-bottom: 2rem;
}

.option {
  padding: 1rem;
  display: block;
  background-color: #e0e7ff; /* Soft blue for options */
  margin-bottom: 0.75rem;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.option:hover {
  background-color: #c7d2fe; /* Slightly darker blue on hover */
}

.option.correct {
  background-color: #28a745; /* Green for correct answers */
}

.option.wrong {
  background-color: #dc3545; /* Red for wrong answers */
}

.option.disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.option input {
  display: none;
}

button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.75rem 1.5rem;
  background-color: #ff6f61; /* Accent color for buttons */
  color: #ffffff; /* White text for contrast */
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.2rem;
  border-radius: 0.5rem;
  transition: background-color 0.3s ease;
}

button:hover:not(:disabled) {
  background-color: #e65b54; /* Slightly darker coral on hover */
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

h2 {
  font-size: 2.5rem;
  margin-bottom: 2rem;
  text-align: center;
  color: #ff6f61; /* Accent color for headings */
}

p {
  color: #666666; /* Medium gray for secondary text */
  font-size: 1.5rem;
  text-align: center;
}* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

body {
  background-color: #f0f4f8; /* Light background for a fresh look */
  color: #333333; /* Dark text for readability */
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  margin: 0; /* Ensure no default margin */
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  width: 100%;
  max-width: 800px; /* Increased max-width for a larger container */
  border-radius: 1rem;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  background-color: #ffffff; /* White background for the quiz container */
}

h1 {
  font-size: 3rem;
  margin-bottom: 2rem;
  color: #ff6f61; /* Vibrant coral color for the title */
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  margin-bottom: 2rem;
  gap: 1rem; /* Add gap between question and score */
}

.quiz-info .question {
  color: #ff6f61; /* Consistent accent color for questions */
  font-size: 1.75rem;
}

.quiz-info .score {
  color: #333333; /* Consistent text color */
  font-size: 1.75rem;
}

.options {
  width: 100%;
  margin-bottom: 2rem;
}

.option {
  padding: 1rem;
  display: block;
  background-color: #e0e7ff; /* Soft blue for options */
  margin-bottom: 0.75rem;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.option:hover {
  background-color: #c7d2fe; /* Slightly darker blue on hover */
}

.option.correct {
  background-color: #28a745; /* Green for correct answers */
}

.option.wrong {
  background-color: #dc3545; /* Red for wrong answers */
}

.option.disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.option input {
  display: none;
}

button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.75rem 1.5rem;
  background-color: #ff6f61; /* Accent color for buttons */
  color: #ffffff; /* White text for contrast */
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.2rem;
  border-radius: 0.5rem;
  transition: background-color 0.3s ease;
}

button:hover:not(:disabled) {
  background-color: #e65b54; /* Slightly darker coral on hover */
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

h2 {
  font-size: 2.5rem;
  margin-bottom: 2rem;
  text-align: center;
  color: #ff6f61; /* Accent color for headings */
}

p {
  color: #666666; /* Medium gray for secondary text */
  font-size: 1.5rem;
  text-align: center;
}
</style>