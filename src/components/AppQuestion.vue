<template>
  <div class="question-wrapper">
    <form @submit.prevent="onSubmit">
      <div class="question">
        <h3
          v-for="(q, i) in question.questions"
          :key="i"
        >
          {{ counter }}
          {{ q }}
        </h3>
        <div
          v-for="(c, n) in question.choices"
          :key="`key-${n}`"
          class="question-radio"
          :class="getClass(n)"
        >
          <input
            v-model="selected"
            :value="n"
            :id="`id-${n}`"
            type="radio"
            name="choices">
          <label :for="`id-${n}`">
            {{ c }}
          </label>
        </div>
      </div>
      <p v-if="!question.answerText" class="question-radio_incorrect">
        There is no answer for this question
      </p>
      <button
        type="submit"
        class="submit">
        Submit
      </button>
    </form>

    <div class="controls">
      <button @click="onControl('prev')">
        Prev
      </button>
      <button @click="onControl('next')">
        Next
      </button>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  name: 'AppQuestion',
  props: {
    question: Object,
    counter: String
  },
  emits: ['submit', 'prev', 'next'],
  setup(props, { emit }) {
    const showAnswer = ref(false)
    const selected = ref()
    const getClass = (n) => {
      if (showAnswer.value && props?.question?.answerText) {
        if (n === props.question.answer) return 'question-radio_correct'
        return 'question-radio_incorrect'
      }
      return ''
    }
    const onSubmit = () => {
      showAnswer.value = true
    }
    const onControl = (e) => {
      showAnswer.value = false
      selected.value = null
      emit(e)
    }
    return {
      selected,
      showAnswer,
      getClass,
      onSubmit,
      onControl,
    }
  }
}
</script>

<style scoped>
.question-wrapper {
  max-width: 500px;
  margin: 50px auto;
}
.question {
  min-height: 200px;
}
.question-radio {
  text-align: left;
}
.question-radio_incorrect {
  color: red;
}
.question-radio_correct {
  color: green;
}
.submit{
  margin: 20px;
}
.controls {
  display: flex;
  justify-content: space-between;
  width: 100%;
  margin: 20px 0;
}

button {
  border: solid 2px transparent;
  min-width: 120px;
  padding: 10px;
  font-size: 18px;
  background: hsl(191, 34%, 54%);
  color: hsl(190deg, 10%, 95%);
  border-radius: 5px;
  cursor: pointer;
}
</style>
