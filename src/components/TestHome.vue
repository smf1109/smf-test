<template>
  <div class="test-home">
    <section class="hero-section">
      <div class="hero-content">
        <div class="hero-icon">🐟</div>
        <h1 class="hero-title">职场摸鱼SBTI测试</h1>
        <p class="hero-subtitle">测测你是哪种职场摸鱼大师？</p>
        
        <div class="hero-stats">
          <div class="stat-item">
            <span class="stat-number">30</span>
            <span class="stat-label">道题目</span>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <span class="stat-number">8</span>
            <span class="stat-label">分钟</span>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <span class="stat-number">20</span>
            <span class="stat-label">种角色</span>
          </div>
        </div>
        
        <button class="start-btn" @click="startTest">
          开始摸鱼测试
        </button>
        <p class="hero-tip">👉 选择最符合你的选项，诚实作答哦！</p>
      </div>
      
      <div class="hero-illustration">
        <div class="office-scene">
          <div class="desk">
            <div class="computer">
              <span class="screen">💻</span>
            </div>
            <div class="coffee">☕</div>
            <div class="snacks">🍪</div>
          </div>
          <div class="fishes">
            <span class="fish" style="animation-delay: 0s;">🐟</span>
            <span class="fish" style="animation-delay: 0.5s;">🐠</span>
            <span class="fish" style="animation-delay: 1s;">🐡</span>
            <span class="fish" style="animation-delay: 1.5s;">🦈</span>
          </div>
        </div>
      </div>
    </section>
    
    <section class="features-section">
      <h2>测试特色</h2>
      <div class="features-grid">
        <article class="feature-card">
          <div class="feature-icon">🎯</div>
          <h3>精准定位</h3>
          <p>30道精心设计的题目，准确判断你的摸鱼类型</p>
        </article>
        <article class="feature-card">
          <div class="feature-icon">🤣</div>
          <h3>幽默有趣</h3>
          <p>轻松搞笑的角色设定，让测试变得有趣好玩</p>
        </article>
        <article class="feature-card">
          <div class="feature-icon">🔒</div>
          <h3>隐私保护</h3>
          <p>测试数据仅保存在本地，放心作答无压力</p>
        </article>
      </div>
    </section>

    <section class="preview-section">
      <h2>部分角色预览</h2>
      <div class="roles-grid">
        <div class="role-card" v-for="role in previewRoles" :key="role.type">
          <div class="role-icon">{{ role.icon }}</div>
          <div class="role-name">{{ role.name }}</div>
        </div>
      </div>
    </section>

    <section class="about-section">
      <h2>关于职场摸鱼SBTI</h2>
      <p class="about-description">
        职场摸鱼SBTI测试是一款有趣的职场性格测试工具，通过30道精心设计的题目，
        帮助你了解自己在工作中的行为模式和性格特点。无论你是职场卷王还是摸鱼宗师，
        都能在这里找到属于你的角色定位！
      </p>
      <div class="about-tags">
        <span class="tag">#职场测试</span>
        <span class="tag">#性格测试</span>
        <span class="tag">#摸鱼文化</span>
        <span class="tag">#职场心理学</span>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const emit = defineEmits(['start'])

const previewRoles = ref([
  { type: 'ZZJ', name: '摸鱼宗师', icon: '🐟' },
  { type: 'DDT', name: '带薪蹲坑王', icon: '🚽' },
  { type: 'TZJ', name: '躺平青年', icon: '🛌' },
  { type: 'XWC', name: '戏精同事', icon: '🎭' },
  { type: 'GLC', name: '吃瓜群众', icon: '🍉' },
  { type: 'ZYJ', name: '职场卷王', icon: '🏆' }
])

const startTest = () => {
  emit('start')
}
</script>

<style scoped>
.test-home {
  min-height: 100vh;
  padding: 60px 20px;
}

.hero-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto 80px;
  gap: 40px;
}

.hero-content {
  flex: 1;
  color: white;
}

.hero-icon {
  font-size: 60px;
  margin-bottom: 20px;
  animation: bounce 2s ease-in-out infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-15px); }
}

.hero-title {
  font-size: 48px;
  font-weight: 700;
  margin-bottom: 20px;
  text-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
  background: linear-gradient(90deg, #FFD700, #FFA500);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-subtitle {
  font-size: 18px;
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 35px;
}

.hero-stats {
  display: flex;
  align-items: center;
  gap: 30px;
  margin-bottom: 40px;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.stat-number {
  font-size: 36px;
  font-weight: 700;
  color: #FFD700;
}

.stat-label {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.8);
}

.stat-divider {
  width: 2px;
  height: 40px;
  background: rgba(255, 255, 255, 0.3);
}

.start-btn {
  padding: 16px 45px;
  background: linear-gradient(135deg, #FFD700, #FFA500);
  color: #333;
  border: none;
  border-radius: 40px;
  font-size: 18px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 8px 25px rgba(255, 215, 0, 0.3);
}

.start-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 30px rgba(255, 215, 0, 0.4);
}

.hero-tip {
  margin-top: 15px;
  font-size: 14px;
  color: rgba(255, 255, 255, 0.7);
}

.hero-illustration {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}

.office-scene {
  position: relative;
  width: 350px;
  height: 300px;
}

.desk {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  background: linear-gradient(180deg, #8B4513, #654321);
  width: 280px;
  height: 30px;
  border-radius: 5px;
}

.computer {
  position: absolute;
  bottom: 35px;
  left: 50%;
  transform: translateX(-50%);
  background: #333;
  width: 150px;
  height: 100px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.screen {
  font-size: 40px;
}

.coffee {
  position: absolute;
  bottom: 40px;
  left: 30px;
  font-size: 25px;
  animation: steam 2s ease-in-out infinite;
}

.snacks {
  position: absolute;
  bottom: 40px;
  right: 30px;
  font-size: 25px;
}

@keyframes steam {
  0%, 100% { opacity: 0.5; }
  50% { opacity: 1; }
}

.fishes {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 200px;
}

.fish {
  position: absolute;
  font-size: 30px;
  animation: swim 3s ease-in-out infinite;
}

.fish:nth-child(1) { top: 20%; left: 10%; }
.fish:nth-child(2) { top: 40%; right: 20%; }
.fish:nth-child(3) { top: 60%; left: 30%; }
.fish:nth-child(4) { top: 30%; right: 5%; }

@keyframes swim {
  0%, 100% { transform: translateX(0) rotate(0deg); }
  25% { transform: translateX(20px) rotate(10deg); }
  50% { transform: translateX(0) rotate(0deg); }
  75% { transform: translateX(-20px) rotate(-10deg); }
}

.features-section {
  max-width: 1200px;
  margin: 0 auto 60px;
  text-align: center;
}

.features-section h2 {
  color: white;
  font-size: 32px;
  margin-bottom: 40px;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
}

.feature-card {
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 35px;
  color: white;
  transition: all 0.3s ease;
}

.feature-card:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-5px);
}

.feature-icon {
  font-size: 40px;
  margin-bottom: 20px;
}

.feature-card h3 {
  font-size: 18px;
  margin-bottom: 12px;
}

.feature-card p {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.6;
}

.preview-section {
  max-width: 1200px;
  margin: 0 auto 60px;
  text-align: center;
}

.preview-section h2 {
  color: white;
  font-size: 28px;
  margin-bottom: 30px;
}

.roles-grid {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 20px;
}

.role-card {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 20px;
  color: white;
  transition: all 0.3s ease;
}

.role-card:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.role-icon {
  font-size: 36px;
  margin-bottom: 10px;
}

.role-name {
  font-size: 14px;
  font-weight: 500;
}

.about-section {
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 40px;
}

.about-section h2 {
  color: white;
  font-size: 28px;
  margin-bottom: 20px;
}

.about-description {
  color: rgba(255, 255, 255, 0.9);
  font-size: 16px;
  line-height: 1.8;
  margin-bottom: 25px;
}

.about-tags {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
}

.tag {
  padding: 8px 18px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 20px;
  font-size: 13px;
  color: white;
}

@media (max-width: 768px) {
  .hero-section {
    flex-direction: column;
    text-align: center;
  }
  
  .hero-title {
    font-size: 32px;
  }
  
  .hero-stats {
    justify-content: center;
  }
  
  .features-grid {
    grid-template-columns: 1fr;
  }
  
  .roles-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>