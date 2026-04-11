<template>
  <div class="app-container">
    <header class="app-header">
      <nav class="app-nav">
        <div class="nav-brand">
          <span class="brand-icon">🐟</span>
          <span class="brand-text">职场摸鱼SBTI</span>
        </div>
      </nav>
    </header>
    
    <main class="app-main">
      <TestHome v-if="currentStep === 'home'" @start="startTest" />
      
      <div v-else-if="currentStep === 'testing'" class="testing-container">
        <QuestionCard
          :question="currentQuestion"
          :currentIndex="currentIndex"
          :total="questions.length"
          v-model="currentAnswer"
          @prev="prevQuestion"
          @next="nextQuestion"
          @complete="showResult"
        />
      </div>
      
      <div v-else-if="currentStep === 'result'" class="result-container">
        <ResultCard :type="resultType" @restart="restartTest" />
      </div>
    </main>
    
    <footer class="app-footer">
      <div class="footer-content">
        <p>职场摸鱼SBTI测试 - 测测你的职场摸鱼角色</p>
        <p class="footer-copyright">© 2026 职场摸鱼SBTI. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import TestHome from './components/TestHome.vue'
import QuestionCard from './components/QuestionCard.vue'
import ResultCard from './components/ResultCard.vue'
import { sbtiQuestions, calculateType } from './data/sbtiData'

const currentStep = ref('home')
const questions = ref(sbtiQuestions)
const currentIndex = ref(0)
const answers = ref({})
const resultType = ref('')

const currentQuestion = computed(() => questions.value[currentIndex.value])

const currentAnswer = computed({
  get: () => answers.value[currentQuestion.value?.id] || '',
  set: (value) => {
    answers.value[currentQuestion.value.id] = value
  }
})

const startTest = () => {
  currentStep.value = 'testing'
  currentIndex.value = 0
  answers.value = {}
  updatePageTitle('职场摸鱼SBTI测试 - 答题中')
}

const prevQuestion = () => {
  if (currentIndex.value > 0) {
    currentIndex.value--
  }
}

const nextQuestion = () => {
  if (currentIndex.value < questions.value.length - 1) {
    currentIndex.value++
  }
}

const showResult = () => {
  const answerArray = Object.values(answers.value)
  resultType.value = calculateType(answerArray)
  currentStep.value = 'result'
  updatePageTitle(`职场摸鱼SBTI测试 - 你的角色是${resultType.value}`)
}

const restartTest = () => {
  currentStep.value = 'home'
  currentIndex.value = 0
  answers.value = {}
  resultType.value = ''
  updatePageTitle('职场摸鱼SBTI测试 - 测测你的职场摸鱼角色')
}

const updatePageTitle = (title) => {
  document.title = title
}

onMounted(() => {
  updatePageTitle('职场摸鱼SBTI测试 - 测测你的职场摸鱼角色')
})
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
  min-height: 100vh;
}

.app-container {
  min-height: 100vh;
  padding-top: 60px;
  padding-bottom: 80px;
}

.app-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  padding: 12px 20px;
  background: rgba(0, 0, 0, 0.15);
  backdrop-filter: blur(15px);
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.1);
}

.app-nav {
  max-width: 1200px;
  margin: 0 auto;
}

.nav-brand {
  display: flex;
  align-items: center;
  gap: 10px;
  color: white;
  font-size: 18px;
  font-weight: 600;
}

.brand-icon {
  font-size: 24px;
}

.app-main {
  min-height: calc(100vh - 140px);
}

.testing-container {
  padding: 40px 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: calc(100vh - 140px);
}

.result-container {
  padding: 40px 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: calc(100vh - 140px);
}

.app-footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 100;
  padding: 18px 20px;
  background: rgba(0, 0, 0, 0.15);
  backdrop-filter: blur(15px);
  text-align: center;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.footer-content {
  color: rgba(255, 255, 255, 0.8);
  font-size: 13px;
}

.footer-copyright {
  margin-top: 4px;
  opacity: 0.6;
}
</style>