<script setup lang="ts">
import { ref, reactive, computed } from 'vue'
import confetti from 'canvas-confetti'
import CountdownTimer from './CountdownTimer.vue'
import QuizResult from './QuizResult.vue'

interface Question {
  id: number
  text: string
  options: string[]
  correctAnswer: number
  explanation: string
  image?: string
  video?: string
  mediaType?: 'image' | 'video' | 'gif'
}

const bypassBirthdayCheck = () => {
  isBeforeBirthday.value = false
}

const isDevelopment = process.env.NODE_ENV === 'development'

// Birthday logic
const birthdayDate = new Date('2025-05-24')
const isBeforeBirthday = ref(true)

isBeforeBirthday.value = false

const handleCountdownComplete = () => {
  isBeforeBirthday.value = false
}

// Replace the existing questions array with this one
const questions = reactive<Question[]>([
  {
    id: 1,
    text: 'Tóc của Goku chuyển sang màu gì khi anh ta "Super Saiyan"?',
    image: '/images/goku-1.jpg', // Image of Super Saiyan Goku
    options: ['Vàng như gà rán', 'Đỏ như ớt', 'Xanh như rau muống', 'Tím như khoai lang'],
    correctAnswer: 0,
    explanation:
      'Tóc vàng như gà rán KFC :)) Hình như họ gọi là "Super Saiyan" gì đó, nhưng nhìn giống như vừa nhuộm tóc ở tiệm cắt tóc rẻ tiền.',
  },
  {
    id: 2,
    text: 'Ông chú tóc nhọn như bông súp lơ này là ai?',
    image: '/images/vegeta-2.webp', // Image of Vegeta
    options: ['Naruto', 'Vegeta', 'Deku', 'Subaru'],
    correctAnswer: 1,
    explanation:
      'Đó là Vegeta! Ông này cả ngày chỉ la "KAKAROT!" và tức tối vì không phải là nhân vật chính. Mà tóc trông như cái súp lơ bị cắm điện ấy.',
  },
  {
    id: 3,
    text: 'Goku và Vegeta có mối quan hệ gì?',
    image: '/images/goku-vegeta-3.jpg', // Image of them together
    options: [
      'Kẻ thù không đội trời chung',
      'Bạn bè thân thiết',
      '"Bạn bè thân thiết" 😉😉',
      'Đồng nghiệp văn phòng',
    ],
    correctAnswer: 2,
    explanation:
      'Theo như các fan thì họ "chỉ là bạn" nhưng cả ngày cứ đuổi theo nhau, cố gắng thu hút sự chú ý của nhau, và đấm nhau. Nghe quen không? 💕',
  },
  {
    id: 4,
    text: 'Goku và Vegeta có thể làm gì khi chạm 2 tay vào nhau?',
    image: '/images/fusion-4.jpg', // Fusion dance image
    options: [
      'Thắng giải Vũ Điệu Đam Mê',
      'Hợp thể thành người mới',
      'Được tuyển vào Bước Nhảy Hoàn Vũ',
      'Làm Thanos nổi giận',
    ],
    correctAnswer: 1,
    explanation:
      'Họ trở thành một người - BÙM - ra một anh chàng mới có tóc của cả hai. Khoa học 9/11 ⚛︎',
  },
  {
    id: 5,
    text: 'Tại sao Vegeta luôn luôn tức giận với Goku?',
    image: '/images/vegeta-angry-5.webp', // Angry Vegeta image
    options: [
      'Vì Goku ăn phần ăn của anh ta',
      'Vì Goku luôn mạnh hơn',
      'Vì Goku mặc đồ cam quá chói mắt',
      'Vì Goku quên sinh nhật của anh ta',
    ],
    correctAnswer: 1,
    explanation:
      'Vegeta là hoàng tử, nhưng Goku - một thằng nông dân - lại mạnh hơn. Tưởng tượng đi, như kiểu Bill Gates tức vì thua trò Candy Crush bởi một ông chú bán vé số 😂',
  },
  {
    id: 6,
    text: 'Làm thế nào để tăng sức mạnh trong Dragon Ball?',
    image: '/images/power-up-6.webp', // Powering up screaming image
    options: [
      'Uống sữa tăng cơ',
      'Thiền định yên lặng',
      'Hét to "AAAAAAAAAAAAA" trong 3 tập phim',
      'Đăng ký gói cước VIP',
    ],
    correctAnswer: 2,
    explanation:
      'HÉT THẬT TO!!! Càng hét to, tóc càng dài, càng đổi màu! 50% thời lượng phim là 2 thằng đứng hét vào không khí. Như kiểu tập thể dục tại nhà (giả sử có tập thể dục)',
  },
  {
    id: 7,
    text: 'Nếu Goku và Vegeta hẹn hò, địa điểm lý tưởng sẽ là đâu?',
    image: '/images/fight-7.webp', // Image of them fighting
    options: [
      'Nhà hàng sang trọng',
      'Rạp chiếu phim lãng mạn',
      'Đấu trường để đánh nhau như điên',
      'Trung tâm thương mại',
    ],
    correctAnswer: 2,
    explanation:
      'Với họ, đánh nhau đến chết là cách thể hiện tình cảm số 1! Quà Valentine? Không. Chocolate? Không. Một cú đấm vào mặt? Perfect. #RelationshipGoals',
  },
  {
    id: 8,
    text: 'Ai là đối thủ tình cảm lớn nhất của Vegeta trong cuộc chiến giành trái tim Goku?',
    image: '/images/piccolo-8.jpg', // Image of Piccolo and Goku
    options: [
      'Chi-Chi (vợ Goku)',
      'Bulma (vợ Vegeta)',
      'Piccolo (anh chàng màu xanh lá)',
      'Frieza (gã trọc đầu trắng)',
    ],
    correctAnswer: 2,
    explanation:
      'Piccolo và Goku cũng có "tình cảm" không kém! Họ nuôi con chung (Gohan), tập luyện cùng nhau, và Piccolo hy sinh vì Goku. Vegeta đang run sợ đấy! 💚',
  },
  {
    id: 9,
    text: 'Rốt cuộc thì vũ trụ Dragon Ball có bao nhiêu tập phim?',
    image: '/images/dbz-series-9.jpg', // Image showing multiple DB series
    options: [
      'Khoảng 50 tập, ngắn gọn',
      'Vài trăm tập, vừa phải',
      'HÀNG NGÀN TẬP không tính các phần phụ',
      'Ít hơn Pokemon',
    ],
    correctAnswer: 2,
    explanation:
      'Dragon Ball, DBZ, GT, Super, Heroes, Kai, phim, game... kể đến năm sau cũng không xong. Mấy thằng này đã chiến đấu từ năm 1984 đến giờ vẫn chưa xong! Có lẽ vì họ dành 5 tập để hét "AAAAA" trước mỗi đòn đánh.',
  },
  {
    id: 10,
    text: 'Điệu nhảy này từ game nào ra? 💀💀💀',
    video: '/videos/vegeta-dance.mp4',
    options: [
      'Fortnite',
      'Free Fire',
      'Just Dance',
      'Bất cứ game nào bắt đầu bằng từ "Dragon Ball"',
    ],
    correctAnswer: 0,
    explanation: 'Đương nhiên là Fortnite :D. Sai cái này thì bro nên đi cạp đất 🐧',
  },
])

const currentQuestionIndex = ref(0)
const selectedAnswer = ref<number | null>(null)
const score = ref(0)
const showExplanation = ref(false)
const quizCompleted = ref(false)
const confettiActive = ref(false)

const currentQuestion = computed(() => questions[currentQuestionIndex.value])

const isCorrect = computed(() => {
  if (selectedAnswer.value === null) return false
  return selectedAnswer.value === currentQuestion.value.correctAnswer
})

// Confetti functions
const createConfetti = () => {
  confetti({
    particleCount: 150,
    spread: 70,
    origin: { y: 0.6 },
    colors: ['#ff0000', '#ffa500', '#ffff00', '#008000', '#0000ff', '#4b0082', '#ee82ee'],
  })
}

const createBigConfetti = () => {
  const end = Date.now() + 2000

  const frame = () => {
    confetti({
      particleCount: 50,
      angle: 60,
      spread: 55,
      origin: { x: 0, y: 0.65 },
      colors: ['#ff0000', '#ffa500', '#ffff00', '#008000', '#0000ff'],
    })

    confetti({
      particleCount: 50,
      angle: 120,
      spread: 55,
      origin: { x: 1, y: 0.65 },
      colors: ['#ff0000', '#ffa500', '#ffff00', '#008000', '#0000ff'],
    })

    if (Date.now() < end) {
      requestAnimationFrame(frame)
    }
  }

  frame()
}

// Update your existing checkAnswer function
const checkAnswer = () => {
  if (selectedAnswer.value === null) return

  showExplanation.value = true

  if (isCorrect.value) {
    score.value++
    confettiActive.value = true
    createConfetti()
    playCorrectSound() // Play correct answer sound
    playConfettiSound() // Play confetti sound
    setTimeout(() => {
      confettiActive.value = false
    }, 3000)
  } else {
    playWrongSound()
  }
}

// Update your existing nextQuestion function
const nextQuestion = () => {
  selectedAnswer.value = null
  showExplanation.value = false

  if (currentQuestionIndex.value < questions.length - 1) {
    currentQuestionIndex.value++
  } else {
    quizCompleted.value = true
    // Show confetti for the end of the quiz
    if (score.value > questions.length / 2) {
      createBigConfetti() // Use the fancier confetti for quiz completion
      playVictorySound() // Play victory sound for completion
    }
  }
}

const restartQuiz = () => {
  currentQuestionIndex.value = 0
  selectedAnswer.value = null
  score.value = 0
  showExplanation.value = false
  quizCompleted.value = false
}

// Add these below your other functions
const playSound = (soundFile: string) => {
  const audio = new Audio(`/sounds/${soundFile}`)
  audio.volume = 0.5 // Adjust volume (0.0 to 1.0)
  audio.play()
}

const playConfettiSound = () => {
  playSound('confetti.mp3')
}

const playCorrectSound = () => {
  playSound('correct.mp3')
}

const playVictorySound = () => {
  playSound('victory.mp3')
}

// Add this with your other sound functions
const playWrongSound = () => {
  playSound('wrong1.mp3')
}
</script>

<template>
  <!-- Show countdown if before birthday -->
  <CountdownTimer
    v-if="isBeforeBirthday"
    :targetDate="birthdayDate"
    :onComplete="handleCountdownComplete"
  />
  <button
    v-if="isDevelopment && isBeforeBirthday"
    @click="bypassBirthdayCheck"
    class="fixed bottom-4 right-4 bg-gray-800 text-white p-2 rounded opacity-50 hover:opacity-100 z-50"
  >
    Debug: Skip Countdown
  </button>
  <!-- Show quiz if it's the birthday or after -->
  <div v-else class="min-h-screen bg-gradient-to-b from-blue-500 to-purple-600 py-10 px-4">
    <div class="max-w-2xl mx-auto bg-white rounded-xl shadow-2xl overflow-hidden">
      <!-- Quiz header -->
      <div class="bg-gradient-to-r from-pink-500 to-purple-500 py-6 px-8">
        <h1 class="text-3xl font-bold text-white text-center">Birthday Quiz Challenge!</h1>
        <p class="text-white text-center mt-2">Test how well you know your boy-love anime 🐧🐧🐧</p>
      </div>

      <!-- Quiz container -->
      <div class="p-8">
        <!-- Quiz in progress -->
        <div v-if="!quizCompleted">
          <!-- Progress bar -->
          <div class="w-full bg-gray-200 rounded-full h-2.5 mb-6">
            <div
              class="bg-purple-600 h-2.5 rounded-full"
              :style="{ width: `${(currentQuestionIndex / questions.length) * 100}%` }"
            ></div>
          </div>

          <!-- Question counter -->
          <div class="text-sm text-gray-600 mb-4">
            Question {{ currentQuestionIndex + 1 }} of {{ questions.length }}
          </div>

          <!-- Current question -->
          <h2 class="text-2xl font-semibold text-gray-800 mb-6">
            {{ currentQuestion.text }}
          </h2>

          <!-- Current question media (image or video) -->
          <div v-if="currentQuestion.image || currentQuestion.video" class="mb-6">
            <!-- Show image if media is image or gif -->
            <img
              v-if="!currentQuestion.video"
              :src="currentQuestion.image"
              alt="Question image"
              class="rounded-lg mx-auto max-h-64 object-contain shadow-md"
            />

            <!-- Show video if media is video -->
            <video
              v-else
              :src="currentQuestion.video"
              autoplay
              loop
              class="rounded-lg mx-auto max-h-64 object-contain shadow-md"
            ></video>
          </div>

          <!-- Answer options -->
          <div class="space-y-3 mb-8">
            <div
              v-for="(option, index) in currentQuestion.options"
              :key="index"
              @click="!showExplanation && (selectedAnswer = index)"
              :class="[
                'p-4 border rounded-lg transition-all',
                // Add cursor-not-allowed when explanation is shown
                showExplanation ? 'cursor-default' : 'cursor-pointer',
                selectedAnswer === index
                  ? 'border-purple-500 bg-purple-50'
                  : 'border-gray-200 hover:border-purple-200',
                showExplanation && index === currentQuestion.correctAnswer
                  ? 'bg-green-100 border-green-500'
                  : '',
                showExplanation &&
                selectedAnswer === index &&
                index !== currentQuestion.correctAnswer
                  ? 'bg-red-100 border-red-500'
                  : '',
              ]"
            >
              <div class="flex items-center">
                <span
                  class="w-6 h-6 flex items-center justify-center rounded-full mr-3"
                  :class="selectedAnswer === index ? 'bg-purple-500 text-white' : 'bg-gray-200'"
                >
                  {{ String.fromCharCode(65 + index) }}
                </span>
                {{ option }}
              </div>
            </div>
          </div>
          <!-- Explanation -->
          <div
            v-if="showExplanation"
            class="mb-8 p-4 rounded-lg"
            :class="isCorrect ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800'"
          >
            <p class="font-medium mb-1">
              {{ isCorrect ? '🎉 Correct!' : '❌ Not quite!' }}
            </p>
            <p>{{ currentQuestion.explanation }}</p>
          </div>

          <!-- Action buttons -->
          <div class="flex justify-between">
            <button
              v-if="!showExplanation"
              @click="checkAnswer"
              :disabled="selectedAnswer === null"
              class="px-6 py-2 bg-purple-600 text-white rounded-lg font-medium disabled:opacity-50 disabled:cursor-not-allowed transition-all"
            >
              Submit
            </button>
            <button
              v-else
              @click="nextQuestion"
              class="px-6 py-2 bg-purple-600 text-white rounded-lg font-medium transition-all"
            >
              {{ currentQuestionIndex < questions.length - 1 ? 'Next Question' : 'See Results' }}
            </button>
          </div>
        </div>

        <!-- Quiz results -->
        <QuizResult
          v-else
          :score="score"
          :totalQuestions="questions.length"
          :onRestart="restartQuiz"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Your existing styles */
.transition-all {
  transition: all 0.3s ease;
}

/* Styles for buttons on hover */
button:not([disabled]):hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
</style>
