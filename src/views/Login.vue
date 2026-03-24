<template>
  <div class="login-container">
    <div class="login-form-wrapper">
      <h1>登录</h1>
      <div class="user-info-note">
        <p><strong>账号信息：</strong></p>
        <p>管理员账号：admin，密码：任意值</p>
        <p>普通用户账号：user，密码：任意值</p>
      </div>
      <a-form
        :model="formState"
        :rules="rules"
        ref="formRef"
        @finish="handleSubmit"
      >
        <a-form-item label="账号" name="username">
          <a-input v-model:value="formState.username" placeholder="请输入账号" />
        </a-form-item>
        <a-form-item label="密码" name="password">
          <a-input-password v-model:value="formState.password" placeholder="请输入密码" />
        </a-form-item>
        <a-form-item>
          <a-button type="primary" html-type="submit" class="login-button">
            登录
          </a-button>
        </a-form-item>
      </a-form>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { message } from 'ant-design-vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const formRef = ref(null)
const formState = reactive({
  username: '',
  password: ''
})

const rules = {
  username: [
    { required: true, message: '请输入账号', trigger: 'blur' }
  ],
  password: [
    { required: true, message: '请输入密码', trigger: 'blur' }
  ]
}

const handleSubmit = () => {
  const users = {
    admin: { role: 'admin' },
    user: { role: 'user' }
  }

  if (users[formState.username]) {
    // 存储用户信息到localStorage
    localStorage.setItem('user', JSON.stringify({
      username: formState.username,
      role: users[formState.username].role
    }))
    message.success('登录成功')
    // 跳转到首页
    router.push('/')
  } else {
    message.error('账号错误')
  }
}
</script>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f5f5f5;
}

.login-form-wrapper {
  width: 400px;
  padding: 30px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.login-form-wrapper h1 {
  text-align: center;
  margin-bottom: 30px;
  color: #1890ff;
}

.user-info-note {
  margin-bottom: 20px;
  padding: 15px;
  background-color: #f0f2f5;
  border-radius: 4px;
  font-size: 14px;
}

.user-info-note p {
  margin: 5px 0;
}

.login-button {
  width: 100%;
}
</style>