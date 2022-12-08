<script setup>
import { ref, computed } from 'vue'
import { quizData } from '../quiz-data.js'

const questions = ref(quizData)
const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected == q.answer){
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

const setAnswer = event => {
	questions.value[currentQuestion.value].selected = event.target.value
	event.target.value = null
}

const nextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
	} else {
		quizCompleted.value = false
	}
}
console.log(questions.value)
</script>

<template>
	<header class="score text-center">
		<h2>{{ score }}/{{ questions.length }}</h2>
	</header>
	<section class="question p-3 text-center">
		<h2>{{ getCurrentQuestion.question }}</h2>
	</section>
	<section class="answer bg-blue-900 flex flex-col items-center p-3">
		<label 
			v-for="(option, index) in getCurrentQuestion.options "
			:key="index"
			:class="`
				option bg-blue-800 
				my-1 text-white
				w-80 p-3 rounded-lg ${
				getCurrentQuestion.selected == index
				? index == getCurrentQuestion.answer
					? 'bg-green-700' // correct answer
					: 'bg-red-600' // wrong answer
				: ''
			} ${
				getCurrentQuestion != null &&
				index != getCurrentQuestion.selected
					? 'disabled'
					: ''
			}`"
			>
			
			<input type="radio"
				:name="getCurrentQuestion.index"
				:value="index"
				v-model="getCurrentQuestion.selected"
				@change="setAnswer">

			<span>{{ option }}</span>
		</label>
	</section>
	<div class="button-wrapper flex-row-reverse w-full">
		<button class="rounded-lg p-2 text-white bg-blue-800">
			Next Question
		</button>
	</div>
</template>