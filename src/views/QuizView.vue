<script setup>
import { ref, watch, computed } from "vue";
import Question from "../components/Question.vue";
import QuizHeader from "../components/QuizHeader.vue"
import quizes from "../data/quizes.json";

import { useRoute } from "vue-router"

const route = useRoute()

const quizId = parseInt(route.params.id);

const quiz = quizes.find(q => q.id === quizId)

const currentQuestionIndex = ref(0);

// One way -> too many lines of code
// // const questionStatus = ref(`${currentQuestionIndex.value}/${quiz.questions.length}`)

// // watch and change the question status once the next button is clicked i.e. the current question index value changes
// watch(() => currentQuestionIndex.value, () => {
// 	questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`
// })

// another way -> shorter line of code
// computed -> compute a value based off on another value i.e. compute questionStatus based off the currentQuestionIndex.value
const questionStatus = computed(() => `${currentQuestionIndex.value}/${quiz.questions.length}`)
const barPercentage = computed(() => `${currentQuestionIndex.value / quiz.questions.length * 100}%`)
</script>

<template>
	<div>
		<QuizHeader :questionStatus="questionStatus" :barPercentage="barPercentage" />

		<div>
			<Question :question="quiz.questions[currentQuestionIndex]" />
		</div>

		<button @click="currentQuestionIndex++">Next Question</button>
	</div>
</template>

<style scoped></style>