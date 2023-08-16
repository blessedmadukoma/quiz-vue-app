<script setup>
import { ref, watch, computed } from "vue";
import Question from "../components/Question.vue";
import QuizHeader from "../components/QuizHeader.vue"
import quizes from "../data/quizes.json";
import Result from "../components/Result.vue"

import { useRoute } from "vue-router"

const route = useRoute()

const quizId = parseInt(route.params.id);

const quiz = quizes.find(q => q.id === quizId)

const currentQuestionIndex = ref(0);

const showResult = ref(false);

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

const numberOfCorrectAnswers = ref(0)

// listen to the emitted event
const onOptionSelected = (isCorrect) => {
	if (isCorrect) {
		numberOfCorrectAnswers.value++;
	}

	if (quiz.questions.length - 1 === currentQuestionIndex.value) {
		showResult.value = true
	}

	currentQuestionIndex.value++
}
</script>

<template>
	<div>
		<QuizHeader :questionStatus="questionStatus" :barPercentage="barPercentage" />

		<div>
			<Question v-if="!showResult" :question="quiz.questions[currentQuestionIndex]" @selectOption="onOptionSelected" />

			<Result v-else :quizQuestionLength="quiz.questions.length" :numberOfCorrectAnswers="numberOfCorrectAnswers" />
		</div>

	</div>
</template>

<style scoped></style>