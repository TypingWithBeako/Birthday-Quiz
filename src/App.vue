<script setup>
import { ref, onMounted } from 'vue'
import QuizComponent from './components/QuizComponent.vue'

const assetsLoaded = ref(false)
const loadingProgress = ref(0)

// List all your assets to preload
const assetsToLoad = [
  // Images
  '/images/goku-1.jpg',
  '/images/vegeta-2.webp',
  '/images/goku-vegeta-3.jpg',
  '/images/fusion-4.jpg',
  '/images/vegeta-angry-5.webp',
  '/images/power-up-6.webp',
  '/images/fight-7.webp',
  '/images/piccolo-8.jpg',
  '/images/dbz-series-9.jpg',

  // Videos
  '/videos/vegeta-dance.mp4',

  // Sounds
  '/sounds/confetti.mp3',
  '/sounds/correct.mp3',
  '/sounds/wrong1.mp3',
  '/sounds/victory.mp3',
]

const preloadAssets = () => {
  let loaded = 0
  const totalAssets = assetsToLoad.length

  assetsToLoad.forEach((asset) => {
    if (asset.endsWith('.mp3')) {
      // Preload audio
      const audio = new Audio()
      audio.addEventListener('canplaythrough', () => {
        loaded++
        loadingProgress.value = Math.floor((loaded / totalAssets) * 100)
        if (loaded === totalAssets) assetsLoaded.value = true
      })
      audio.src = asset
    } else if (asset.endsWith('.mp4') || asset.endsWith('.webm')) {
      // Preload video
      const video = document.createElement('video')
      video.addEventListener('canplaythrough', () => {
        loaded++
        loadingProgress.value = Math.floor((loaded / totalAssets) * 100)
        if (loaded === totalAssets) assetsLoaded.value = true
      })
      video.src = asset
    } else {
      // Preload image
      const img = new Image()
      img.onload = () => {
        loaded++
        loadingProgress.value = Math.floor((loaded / totalAssets) * 100)
        if (loaded === totalAssets) assetsLoaded.value = true
      }
      img.src = asset
    }
  })
}

onMounted(() => {
  preloadAssets()
})
</script>

<template>
  <!-- Loading screen -->
  <div v-if="!assetsLoaded" class="preloader">
    <div class="loading-container">
      <h2 class="loading-text">Loading Birthday Quiz...</h2>
      <div class="progress-bar">
        <div class="progress-fill" :style="{ width: `${loadingProgress}%` }"></div>
      </div>
      <p class="loading-percent">{{ loadingProgress }}%</p>
    </div>
  </div>

  <!-- Main app when assets are loaded -->
  <QuizComponent v-else />
</template>

<style scoped>
.preloader {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background: linear-gradient(-45deg, #3b82f6, #8b5cf6, #6366f1, #4f46e5);
  background-size: 400% 400%;
  animation: gradient-shift 15s ease infinite;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.loading-container {
  text-align: center;
  padding: 2rem;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border-radius: 1rem;
  max-width: 80%;
}

.loading-text {
  color: white;
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.progress-bar {
  width: 100%;
  height: 20px;
  background: rgba(255, 255, 255, 0.3);
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 0.5rem;
}

.progress-fill {
  height: 100%;
  background: white;
  border-radius: 10px;
  transition: width 0.3s ease;
}

.loading-percent {
  color: white;
  font-size: 1rem;
}

@keyframes gradient-shift {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
</style>
