<template>
  <div class="home-container" v-if="user">
    <div class="user-info">
      <p>角色: {{ user.role }}</p>
      <a-button type="primary" @click="showLogoutModal">退出登录</a-button>
    </div>
    <div class="role-content">
      <div class="table-box">
        <a-table :columns="columns" :data-source="tableData" row-key="id">
          <template #overtime="{ text }">
            <a-tag :color="text ? 'red' : 'green'">
              {{ text ? 'Yes' : 'No' }}
            </a-tag>
          </template>
          <template #action="{ record }">
            <a-button 
              v-if="user.role === 'admin'" 
              danger 
              @click="showDeleteModal(record.id)"
            >
              Delete
            </a-button>
          </template>
        </a-table>
      </div>
      <div class="charts-box">
        <div ref="chartRef" class="chart-container"></div>
      </div>
    </div>
    
    <!-- 退出登录确认模态框 -->
    <a-modal
      v-model:visible="logoutModalVisible"
      title="退出登录确认"
      @ok="handleLogout"
      @cancel="logoutModalVisible = false"
    >
      <p>确定要退出登录吗？</p>
    </a-modal>
    
    <!-- 删除确认模态框 -->
    <a-modal
      v-model:visible="deleteModalVisible"
      title="删除确认"
      @ok="confirmDelete"
      @cancel="deleteModalVisible = false"
    >
      <p>确定要删除这条记录吗？</p>
    </a-modal>
  </div>
</template>

<script setup>
  import { ref, onMounted, watch } from 'vue'
  import { useRouter } from 'vue-router'
  import { message, Modal } from 'ant-design-vue'
  import * as echarts from 'echarts'

  const router = useRouter()
  const user = ref(null)
  const chartRef = ref(null)
  let chartInstance = null
  
  // 模态框状态
  const logoutModalVisible = ref(false)
  const deleteModalVisible = ref(false)
  const currentDeleteId = ref(null)
  
  // 表格数据
  const tableData = ref([
    {
      "id": "001",
      "project": "Road Project A",
      "overtime": true,
      "hours": 3.5,
      "created_at": "2024-04-10 10:30"
    },
    {
      "id": "002",
      "project": "Bridge Maintenance B",
      "overtime": false,
      "hours": 2,
      "created_at": "2024-04-09 13:00"
    },
    {
      "id": "003",
      "project": "Pipeline Fix C",
      "overtime": true,
      "hours": 4.5,
      "created_at": "2024-04-08 08:00"
    },
    {
      "id": "004",
      "project": "Bridge Maintenance B",
      "overtime": true,
      "hours": 3,
      "created_at": "2024-04-07 16:45"
    },
    {
      "id": "005",
      "project": "Tunnel Cleaning D",
      "overtime": false,
      "hours": 8.1,
      "created_at": "2024-04-03 11:43"
    }
  ])
  
  // 表格列配置
  const columns = [
    {
      title: 'ID',
      dataIndex: 'id',
      key: 'id'
    },
    {
      title: 'Project',
      dataIndex: 'project',
      key: 'project'
    },
    {
      title: 'Overtime',
      dataIndex: 'overtime',
      key: 'overtime',
      slots: {
        customRender: 'overtime'
      }
    },
    {
      title: 'Hours',
      dataIndex: 'hours',
      key: 'hours'
    },
    {
      title: 'Created At',
      dataIndex: 'created_at',
      key: 'created_at'
    },
    {
      title: 'Action',
      key: 'action',
      slots: {
        customRender: 'action'
      }
    }
  ]

  // 初始化图表
  const initChart = () => {
    if (chartRef.value) {
      chartInstance = echarts.init(chartRef.value)
      updateChart()
    }
  }
  
  // 更新图表
  const updateChart = () => {
    if (!chartInstance) return
    
    const xAxisData = tableData.value.map(item => item.project)
    const seriesData = tableData.value.map(item => item.hours)
    
    const option = {
      title: {
        left: 'center'
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'shadow'
        }
      },
      xAxis: {
        type: 'category',
        data: xAxisData,
      },
      yAxis: {
        type: 'value',
        name: 'Hours'
      },
      series: [
        {
          name: 'Hours',
          type: 'bar',
          data: seriesData,
        }
      ]
    }
    
    chartInstance.setOption(option)
  }

  // 检查本地存储中是否有用户信息
  onMounted(() => {
    const storedUser = localStorage.getItem('user')
    if (storedUser) {
      user.value = JSON.parse(storedUser)
      // 初始化图表
      setTimeout(initChart, 100)
    } else {
      // 如果没有用户信息，跳转到登录页
      router.push('/login')
    }
  })

  // 监听表格数据变化，更新图表
  watch(tableData, () => {
    updateChart()
  }, { deep: true })

  // 显示退出登录模态框
  const showLogoutModal = () => {
    logoutModalVisible.value = true
  }
  
  // 处理退出登录
  const handleLogout = () => {
    localStorage.removeItem('user')
    router.push('/login')
  }
  
  // 显示删除模态框
  const showDeleteModal = (id) => {
    currentDeleteId.value = id
    deleteModalVisible.value = true
  }
  
  // 确认删除
  const confirmDelete = () => {
    if (currentDeleteId.value) {
      tableData.value = tableData.value.filter(item => item.id !== currentDeleteId.value)
      message.success('Deleted successfully')
      deleteModalVisible.value = false
      currentDeleteId.value = null
    }
  }
</script>

<style scoped lang="less">
  .home-container {
    min-height: 100vh;
    padding: 20px;
  }

  .user-info {
    margin-bottom: 30px;
    padding: 20px;
    background-color: #f5f5f5;
    border-radius: 8px;
    display: flex;
    justify-content: space-between;
  }

  .role-content {
    display: flex;
    justify-content: space-between;
    .table-box,
    .charts-box {
      width: calc(50% - 20px);
      background-color: white;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
      height: 500px;
    }
    .chart-container {
      width: 100%;
      height: 100%;
    }
  }
</style>