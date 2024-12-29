<template>
    <div class="question">
        <h3> {{ question.question }}</h3>
        <ul>
            <li v-for="(choice, index) in randomChoices" :key="choice">
                    <!---<Answer
                    :id="`answer${index}`"
                    :disabled="hasAnswer"
                    :value="choice"
                    v-model="answer"
                    :correctAnswer="question.correct_answer"
                    />-->
                    <Answer
                    :id="`answer${index}`"
                    :disabled="hasAnswer"
                    :value="choice"
                    @change="onAnswer"
                    :correctAnswer="question.correct_answer"
                    />
            </li>
        </ul>
        <!---<button :disabled="!hasAnswer" @click="emits('answer',answer)">Question suivante </button>--->
        
    </div>

</template>

<script setup>
import { shuffleArray } from '@/functions/array';
import { ref, computed, watch, onUnmounted, onMounted } from 'vue';
import Answer from './Answer.vue';
const props = defineProps({
    question: Object
})
const answer = ref(null)
const emits = defineEmits(['answer'])
const hasAnswer = computed(() => answer.value !== null)
const randomChoices = computed(() => shuffleArray(props.question.choices))
/*watch(() => props.question, () => {
    answer.value = null
})*/

let timer
onMounted(() => {
    timer = setTimeout(() => {
            emits('answer',answer.value)
    }, 3_000)
})

onUnmounted(() => {
    clearTimeout(timer)
})

const onAnswer = (e) => {
    answer.value = e.currentTarget.value
    clearTimeout(timer)
    timer = setTimeout(() => {
        emits('answer',answer.value)
    },1_000)
}
</script>

<style>

.question {
    padding-top: 2rem;
}

.question button {
    margin-left: auto;
    display: block;
}
</style>