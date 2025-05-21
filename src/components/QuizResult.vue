<script setup lang="ts">
const props = defineProps<{
  score: number
  totalQuestions: number
  onRestart: () => void
}>()

const getResultMessage = () => {
  if (props.score === props.totalQuestions) {
    return 'Brotherman nghiện Dragon Ball vl 💀💀💀'
  } else if (props.score >= props.totalQuestions * 0.7) {
    return 'Bruh không được full câu hỏi là gà đấy bro 🐧🐧🐧'
  } else if (props.score >= props.totalQuestions * 0.5) {
    return 'Bro còn nhớ Go/jo à (💔💔💔)'
  } else {
    return 'Chuyển hộ khẩu sang Re:Zero luôn đi bro :D'
  }
}
</script>

<template>
  <div class="text-center py-8">
    <h2 class="text-3xl font-bold text-gray-800 mb-4">Quiz Complete!</h2>
    <p class="text-xl mb-4">
      Your score:
      <span class="font-bold text-purple-600">{{ score }}/{{ totalQuestions }}</span>
    </p>

    <div class="mb-8">
      <p
        :class="
          score === totalQuestions
            ? 'text-green-600 font-medium'
            : score >= totalQuestions * 0.7
              ? 'text-green-600 font-medium'
              : score >= totalQuestions * 0.5
                ? 'text-yellow-600 font-medium'
                : 'text-red-600 font-medium'
        "
      >
        {{ getResultMessage() }}
      </p>
    </div>

    <button
      @click="onRestart"
      class="px-6 py-2 bg-purple-600 text-white rounded-lg font-medium transition-all hover:transform hover:translate-y-[-2px] hover:shadow-md"
    >
      Try Again
    </button>
  </div>
</template>
