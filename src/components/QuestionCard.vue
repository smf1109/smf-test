<template>
  <div class="question-card">
    <div class="question-header">
      <span class="question-number">{{ currentIndex + 1 }} / {{ total }}</span>
      <div class="progress-bar">
        <div 
          class="progress-fill" 
          :style="{ width: `${((currentIndex + 1) / total) * 100}%` }"
        ></div>
      </div>
    </div>
    
    <div class="question-content">
      <h2 class="question-text">{{ question.question }}</h2>
      
      <div class="options-grid">
        <button
          v-for="option in question.options"
          :key="option.value"
          class="option-btn"
          :class="{ active: selectedOption === option.value }"
          @click="selectOption(option.value)"
        >
          <span class="option-label">{{ option.value }}</span>
          <span class="option-text">{{ option.label }}</span>
        </button>
      </div>
    </div>
    
    <div class="question-footer">
      <button
        class="nav-btn prev-btn"
        :disabled="currentIndex === 0"
        @click="$emit('prev')"
      >
        <span>上一题</span>
      </button>
      <button
        class="nav-btn next-btn"
        :disabled="!selectedOption"
        @click="handleNext"
      >
        <span>{{ currentIndex === total - 1 ? '查看结果' : '下一题' }}</span>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  question: {
    type: Object,
    required: true
  },
  currentIndex: {
    type: Number,
    required: true
  },
  total: {
    type: Number,
    required: true
  },
  modelValue: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['update:modelValue', 'prev', 'next', 'complete'])

const selectedOption = ref(props.modelValue)

watch(() => props.modelValue, (val) => {
  selectedOption.value = val
})

const selectOption = (value) => {
  selectedOption.value = value
  emit('update:modelValue', value)
}

const handleNext = () => {
  if (props.currentIndex === props.total - 1) {
    emit('complete')
  } else {
    emit('next')
  }
}
</script>

<style scoped>
.question-card {
  background: white;
  border-radius: 20px;
  padding: 30px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15);
  max-width: 700px;
  margin: 0 auto;
}

.question-header {
  margin-bottom: 30px;
}

.question-number {
  font-size: 14px;
  color: #666;
  font-weight: 500;
}

.progress-bar {
  height: 8px;
  background: #f0f0f0;
  border-radius: 4px;
  margin-top: 15px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #FFD700, #FFA500);
  border-radius: 4px;
  transition: width 0.3s ease;
}

.question-content {
  margin-bottom: 30px;
}

.question-text {
  font-size: 20px;
  color: #333;
  margin-bottom: 25px;
  font-weight: 600;
  text-align: center;
}

.options-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 15px;
}

.option-btn {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 20px;
  border: 2px solid #e0e0e0;
  border-radius: 15px;
  background: white;
  cursor: pointer;
  transition: all 0.3s ease;
  text-align: left;
}

.option-btn:hover {
  border-color: #FFD700;
  background: #fffef0;
}

.option-btn.active {
  border-color: #FFD700;
  background: linear-gradient(135deg, #FFF8DC 0%, #FFE4B5 100%);
}

.option-label {
  font-size: 18px;
  font-weight: 700;
  color: #FF8C00;
  margin-bottom: 8px;
}

.option-text {
  font-size: 15px;
  color: #555;
  line-height: 1.5;
}

.question-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav-btn {
  padding: 14px 35px;
  border-radius: 35px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  border: none;
}

.prev-btn {
  background: #f5f5f5;
  color: #666;
}

.prev-btn:hover:not(:disabled) {
  background: #e8e8e8;
}

.prev-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.next-btn {
  background: linear-gradient(135deg, #FFD700 0%, #FFA500 100%);
  color: #333;
}

.next-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(255, 215, 0, 0.4);
}

.next-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

@media (max-width: 600px) {
  .options-grid {
    grid-template-columns: 1fr;
  }
}
</style>