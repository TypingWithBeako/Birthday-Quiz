<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import confetti from 'canvas-confetti'

const props = defineProps<{
  targetDate: Date
  onComplete: () => void
}>()

const countdown = ref({
  days: 0,
  hours: 0,
  minutes: 0,
  seconds: 0,
})

let countdownInterval: ReturnType<typeof setInterval> | null = null

const playSound = (soundFile: string) => {
  const audio = new Audio(`/sounds/${soundFile}`)
  audio.volume = 0.5 // Adjust volume (0.0 to 1.0)
  audio.play()
}

const calculateTimeRemaining = () => {
  const now = new Date().getTime()
  const targetTime = props.targetDate.getTime()
  const timeRemaining = targetTime - now

  if (timeRemaining <= 0) {
    if (countdownInterval) clearInterval(countdownInterval)
    props.onComplete()
    return
  }

  // Calculate remaining time
  countdown.value.days = Math.floor(timeRemaining / (1000 * 60 * 60 * 24))
  countdown.value.hours = Math.floor((timeRemaining % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
  countdown.value.minutes = Math.floor((timeRemaining % (1000 * 60 * 60)) / (1000 * 60))
  countdown.value.seconds = Math.floor((timeRemaining % (1000 * 60)) / 1000)
}

// Function to create birthday confetti
const createBirthdayParticles = (event: MouseEvent) => {
  const colors = ['#FF9999', '#FF99CC', '#CC99FF', '#9999FF', '#99CCFF', '#99FFCC', '#CCFF99']

  const origin = {
    x: event.clientX / window.innerWidth,
    y: event.clientY / window.innerHeight,
  }

  confetti({
    particleCount: 100,
    spread: 70,
    origin,
    colors,
    shapes: ['circle', 'square'],
    gravity: 0.8,
    scalar: 1.2,
  })
  playSound('confetti.mp3')
}

onMounted(() => {
  calculateTimeRemaining()
  countdownInterval = setInterval(calculateTimeRemaining, 1000)
})

onUnmounted(() => {
  if (countdownInterval) clearInterval(countdownInterval)
})
</script>

<template>
  <div class="countdown-container" @click="createBirthdayParticles">
    <div class="birthday-background"></div>

    <div class="text-center p-8 max-w-2xl z-10 relative">
      <h1 class="text-4xl font-bold text-white mb-8 glow-text">🎁 Birthday Quiz Locked! 🔒</h1>
      <p class="text-xl text-white mb-12">Đây là quà sinh nhật của bro. Chưa đến lúc mở đâu!</p>

      <div class="flex justify-center gap-4 mb-8">
        <div class="countdown-box">
          <div class="text-3xl font-bold text-purple-600">{{ countdown.days }}</div>
          <div class="text-gray-600 text-sm">Ngày</div>
        </div>
        <div class="countdown-box">
          <div class="text-3xl font-bold text-purple-600">{{ countdown.hours }}</div>
          <div class="text-gray-600 text-sm">Giờ</div>
        </div>
        <div class="countdown-box">
          <div class="text-3xl font-bold text-purple-600">{{ countdown.minutes }}</div>
          <div class="text-gray-600 text-sm">Phút</div>
        </div>
        <div class="countdown-box">
          <div class="text-3xl font-bold text-purple-600">{{ countdown.seconds }}</div>
          <div class="text-gray-600 text-sm">Giây</div>
        </div>
      </div>

      <p class="text-white text-lg animate-pulse">Quay lại vào đúng sinh nhật bro nhé! 🎂</p>
      <p class="text-white text-sm mt-6 cursor-hint">Click anywhere to celebrate early!</p>
    </div>
  </div>
</template>

<style scoped>
.countdown-container {
  position: fixed; /* Change from relative to fixed */
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  z-index: 50; /* Higher z-index to ensure it's above everything */
  overflow: hidden; /* Prevent scrolling */
  display: flex;
  align-items: center;
  justify-content: center;
}

.birthday-background {
  position: absolute;
  top: -50%;
  left: -50%;
  right: -50%;
  bottom: -50%;
  width: 200%;
  height: 200%;
  background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
  background-size: 400% 400%;
  animation: gradient-shift 15s ease infinite;
  transform-origin: center center;
  z-index: 0;
  background: linear-gradient(-45deg, #ff9933, #e73c7e, #4c8bf5, #ff7733);
  background-size: 400% 400%;
  animation: gradient-shift 25s ease infinite;
  transform-origin: center center;
  z-index: 0;
}

.countdown-box {
  background-color: white;
  border-radius: 0.75rem;
  padding: 1rem;
  width: 5rem;
  text-align: center;
  transition: all 0.3s ease;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  position: relative;
  overflow: hidden;
}

.countdown-box::before {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.8) 0%, rgba(255, 255, 255, 0) 70%);
  opacity: 0;
  transition: opacity 0.5s ease;
}

.countdown-box:hover {
  transform: translateY(-10px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.countdown-box:hover::before {
  opacity: 1;
  animation: shimmer 1.5s infinite;
}

.glow-text {
  text-shadow: 0 0 10px rgba(255, 255, 255, 0.7);
  animation: glow 2s infinite alternate;
}

.cursor-hint {
  animation: bounce 1s ease infinite alternate;
  opacity: 0.7;
}

@keyframes gradient-shift {
  0% {
    background-position: 0% 50%;
    transform: rotate(0deg);
  }
  50% {
    background-position: 100% 50%;
    transform: rotate(3deg);
  }
  100% {
    background-position: 0% 50%;
    transform: rotate(0deg);
  }
}

@keyframes pulse {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}

@keyframes glow {
  from {
    text-shadow: 0 0 10px rgba(255, 255, 255, 0.7);
  }
  to {
    text-shadow:
      0 0 20px rgba(255, 255, 255, 1),
      0 0 30px #ff77bc;
  }
}

@keyframes shimmer {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes bounce {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(-5px);
  }
}

.animate-pulse {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}
</style>
