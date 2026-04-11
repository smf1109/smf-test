<template>
  <div class="result-card">
    <div class="result-header">
      <div class="type-icon">{{ sbtiType.icon }}</div>
      <div class="type-badge">{{ type }}</div>
      <h1 class="type-name">{{ sbtiType.name }}</h1>
    </div>
    
    <div class="result-content">
      <p class="type-description">{{ sbtiType.description }}</p>
      
      <div class="traits-section">
        <h3>你的摸鱼特质</h3>
        <div class="traits-list">
          <span 
            v-for="trait in sbtiType.traits" 
            :key="trait" 
            class="trait-tag"
          >
            {{ trait }}
          </span>
        </div>
      </div>

      <div class="analysis-section">
        <div class="analysis-card strengths-card">
          <div class="analysis-icon">💪</div>
          <div class="analysis-content">
            <h4>优势</h4>
            <ul>
              <li v-for="item in sbtiType.analysis.strengths" :key="item">{{ item }}</li>
            </ul>
          </div>
        </div>

        <div class="analysis-card weaknesses-card">
          <div class="analysis-icon">⚠️</div>
          <div class="analysis-content">
            <h4>待改进</h4>
            <ul>
              <li v-for="item in sbtiType.analysis.weaknesses" :key="item">{{ item }}</li>
            </ul>
          </div>
        </div>

        <div class="analysis-card jobs-card">
          <div class="analysis-icon">💼</div>
          <div class="analysis-content">
            <h4>适合岗位</h4>
            <ul>
              <li v-for="item in sbtiType.analysis.suitableJobs" :key="item">{{ item }}</li>
            </ul>
          </div>
        </div>

        <div class="analysis-card tips-card">
          <div class="analysis-icon">💡</div>
          <div class="analysis-content">
            <h4>成长建议</h4>
            <p>{{ sbtiType.analysis.tips }}</p>
          </div>
        </div>
      </div>

      <div class="funfact-section">
        <div class="funfact-icon">🤣</div>
        <p class="funfact-text">{{ sbtiType.analysis.funFact }}</p>
      </div>
      
      <div class="celebration-section">
        <div class="celebration-emoji">🎉</div>
        <p>恭喜你获得 "{{ sbtiType.name }}" 称号！</p>
      </div>
    </div>
    
    <div class="result-footer">
      <button class="restart-btn" @click="$emit('restart')">
        再测一次
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { sbtiTypes } from '../data/sbtiData'

const props = defineProps({
  type: {
    type: String,
    required: true
  }
})

defineEmits(['restart'])

const sbtiType = computed(() => {
  return sbtiTypes[props.type] || {
    name: '未知角色',
    description: '暂无描述',
    traits: [],
    icon: '❓',
    analysis: {
      strengths: [],
      weaknesses: [],
      suitableJobs: [],
      tips: '',
      funFact: ''
    }
  }
})
</script>

<style scoped>
.result-card {
  background: white;
  border-radius: 20px;
  padding: 40px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15);
  max-width: 700px;
  margin: 0 auto;
  max-height: 75vh;
  overflow: auto;
  /* 隐藏滚动条 */
  ::-webkit-scrollbar {
    display: none !important;
  }
}

.result-header {
  text-align: center;
  margin-bottom: 35px;
}

.type-icon {
  font-size: 80px;
  margin-bottom: 15px;
  animation: bounce 1s ease-in-out infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

.type-badge {
  display: inline-block;
  padding: 10px 25px;
  background: linear-gradient(135deg, #FFD700 0%, #FFA500 100%);
  color: #333;
  font-size: 18px;
  font-weight: 700;
  border-radius: 50px;
  margin-bottom: 15px;
  letter-spacing: 2px;
}

.type-name {
  font-size: 28px;
  color: #333;
  margin: 0;
  font-weight: 600;
}

.result-content {
  margin-bottom: 35px;
}

.type-description {
  font-size: 16px;
  color: #666;
  line-height: 1.8;
  text-align: center;
  margin-bottom: 30px;
}

.traits-section {
  margin-bottom: 30px;
}

.traits-section h3 {
  font-size: 16px;
  color: #888;
  margin-bottom: 15px;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.traits-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.trait-tag {
  padding: 10px 22px;
  background: linear-gradient(135deg, #FFB6C1 0%, #FF69B4 100%);
  color: white;
  border-radius: 25px;
  font-size: 14px;
  font-weight: 500;
}

.analysis-section {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 15px;
  margin-bottom: 25px;
}

.analysis-card {
  background: #f8f9fa;
  border-radius: 15px;
  padding: 20px;
  display: flex;
  gap: 15px;
}

.analysis-icon {
  font-size: 28px;
  flex-shrink: 0;
}

.analysis-content h4 {
  font-size: 14px;
  color: #888;
  margin-bottom: 10px;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.analysis-content ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

.analysis-content li {
  font-size: 13px;
  color: #555;
  padding: 4px 0;
  padding-left: 15px;
  position: relative;
}

.analysis-content li::before {
  content: '✓';
  position: absolute;
  left: 0;
  color: #667eea;
  font-size: 12px;
}

.analysis-content p {
  font-size: 13px;
  color: #555;
  line-height: 1.6;
  margin: 0;
}

.strengths-card {
  border-top: 3px solid #4CAF50;
}

.weaknesses-card {
  border-top: 3px solid #FF9800;
}

.jobs-card {
  border-top: 3px solid #2196F3;
}

.tips-card {
  grid-column: span 2;
  border-top: 3px solid #9C27B0;
}

.funfact-section {
  background: linear-gradient(135deg, #FFF3E0 0%, #FFE0B2 100%);
  border-radius: 15px;
  padding: 20px;
  display: flex;
  gap: 15px;
  margin-bottom: 25px;
}

.funfact-icon {
  font-size: 32px;
  flex-shrink: 0;
}

.funfact-text {
  font-size: 14px;
  color: #E65100;
  line-height: 1.6;
  margin: 0;
  font-style: italic;
}

.celebration-section {
  background: linear-gradient(135deg, #FFF8DC 0%, #FFE4B5 100%);
  border-radius: 15px;
  padding: 25px;
  text-align: center;
}

.celebration-emoji {
  font-size: 40px;
  margin-bottom: 10px;
}

.celebration-section p {
  font-size: 16px;
  color: #8B4513;
  font-weight: 500;
  margin: 0;
}

.result-footer {
  text-align: center;
}

.restart-btn {
  padding: 15px 50px;
  background: linear-gradient(135deg, #FFD700 0%, #FFA500 100%);
  color: #333;
  border: none;
  border-radius: 35px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.restart-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 25px rgba(255, 215, 0, 0.4);
}

@media (max-width: 600px) {
  .analysis-section {
    grid-template-columns: 1fr;
  }
  
  .tips-card {
    grid-column: span 1;
  }
}
</style>