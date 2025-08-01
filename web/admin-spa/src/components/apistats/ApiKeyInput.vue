<template>
  <div class="api-input-wide-card glass-strong rounded-3xl p-6 mb-8 shadow-xl">
    <!-- 标题区域 -->
    <div class="wide-card-title text-center mb-6">
      <h2 class="text-2xl font-bold mb-2">
        <i class="fas fa-chart-line mr-3" />
        使用统计查询
      </h2>
      <p class="text-base text-gray-600">
        查询您的 API Key 使用情况和统计数据
      </p>
    </div>
    
    <!-- 输入区域 -->
    <div class="max-w-4xl mx-auto">
      <div class="api-input-grid grid grid-cols-1 lg:grid-cols-4">
        <!-- API Key 输入 -->
        <div class="lg:col-span-3">
          <label class="block text-sm font-medium mb-2 text-gray-700">
            <i class="fas fa-key mr-2" />
            输入您的 API Key
          </label>
          <input 
            v-model="apiKey" 
            type="password" 
            placeholder="请输入您的 API Key (cr_...)"
            class="wide-card-input w-full"
            :disabled="loading"
            @keyup.enter="queryStats"
          >
        </div>
        
        <!-- 查询按钮 -->
        <div class="lg:col-span-1">
          <label class="hidden lg:block text-sm font-medium mb-2 text-gray-700">
            &nbsp;
          </label>
          <button 
            :disabled="loading || !apiKey.trim()"
            class="btn btn-primary btn-query w-full h-full flex items-center justify-center gap-2"
            @click="queryStats"
          >
            <i
              v-if="loading"
              class="fas fa-spinner loading-spinner"
            />
            <i
              v-else
              class="fas fa-search"
            />
            {{ loading ? '查询中...' : '查询统计' }}
          </button>
        </div>
      </div>
      
      <!-- 安全提示 -->
      <div class="security-notice mt-4">
        <i class="fas fa-shield-alt mr-2" />
        您的 API Key 仅用于查询自己的统计数据，不会被存储或用于其他用途
      </div>
    </div>
  </div>
</template>

<script setup>
import { storeToRefs } from 'pinia'
import { useApiStatsStore } from '@/stores/apistats'

const apiStatsStore = useApiStatsStore()
const { apiKey, loading } = storeToRefs(apiStatsStore)
const { queryStats } = apiStatsStore
</script>

<style scoped>
/* 宽卡片样式 */
.api-input-wide-card {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(25px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  box-shadow: 
    0 25px 50px -12px rgba(0, 0, 0, 0.25),
    0 0 0 1px rgba(255, 255, 255, 0.05),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.api-input-wide-card:hover {
  box-shadow: 
    0 32px 64px -12px rgba(0, 0, 0, 0.35),
    0 0 0 1px rgba(255, 255, 255, 0.08),
    inset 0 1px 0 rgba(255, 255, 255, 0.15);
  transform: translateY(-1px);
}

/* 标题样式 */
.wide-card-title h2 {
  color: #1f2937;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  font-weight: 700;
}

.wide-card-title p {
  color: #4b5563;
  text-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
}

.wide-card-title .fas.fa-chart-line {
  color: #3b82f6;
  text-shadow: 0 1px 2px rgba(59, 130, 246, 0.2);
}

/* 网格布局 */
.api-input-grid {
  align-items: end;
  gap: 1rem;
}

/* 输入框样式 */
.wide-card-input {
  background: rgba(255, 255, 255, 0.95);
  border: 2px solid rgba(255, 255, 255, 0.4);
  border-radius: 12px;
  padding: 14px 16px;
  font-size: 16px;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
  color: #1f2937;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.wide-card-input::placeholder {
  color: #9ca3af;
}

.wide-card-input:focus {
  outline: none;
  border-color: #60a5fa;
  box-shadow: 
    0 0 0 3px rgba(96, 165, 250, 0.2),
    0 10px 15px -3px rgba(0, 0, 0, 0.1);
  background: white;
}

/* 按钮样式 */
.btn {
  font-weight: 500;
  border-radius: 12px;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
  letter-spacing: 0.025em;
}

/* 查询按钮特定样式 */
.btn-query {
  padding: 14px 24px;
  font-size: 16px;
}

.btn-primary {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  box-shadow: 
    0 10px 15px -3px rgba(102, 126, 234, 0.3),
    0 4px 6px -2px rgba(102, 126, 234, 0.05);
}

.btn-primary:hover:not(:disabled) {
  transform: translateY(-1px);
  box-shadow: 
    0 20px 25px -5px rgba(102, 126, 234, 0.3),
    0 10px 10px -5px rgba(102, 126, 234, 0.1);
}

.btn-primary:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

/* 安全提示样式 */
.security-notice {
  background: rgba(255, 255, 255, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.25);
  backdrop-filter: blur(10px);
  border-radius: 8px;
  padding: 12px 16px;
  color: #374151;
  font-size: 0.875rem;
  transition: all 0.3s ease;
}

.security-notice:hover {
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.35);
}

.security-notice .fas.fa-shield-alt {
  color: #10b981;
  text-shadow: 0 1px 2px rgba(16, 185, 129, 0.2);
}

/* 加载动画 */
.loading-spinner {
  animation: spin 1s linear infinite;
  filter: drop-shadow(0 0 4px rgba(255, 255, 255, 0.5));
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

/* 响应式优化 */
@media (max-width: 768px) {
  .api-input-wide-card {
    padding: 1.25rem;
  }
  
  .wide-card-title {
    margin-bottom: 1.25rem;
  }
  
  .wide-card-title h2 {
    font-size: 1.5rem;
  }
  
  .wide-card-title p {
    font-size: 0.875rem;
  }
  
  .api-input-grid {
    gap: 1rem;
  }
  
  .wide-card-input {
    padding: 12px 14px;
    font-size: 15px;
  }
  
  .btn-query {
    padding: 12px 20px;
    font-size: 15px;
  }
  
  .security-notice {
    padding: 10px 14px;
    font-size: 0.8rem;
  }
}

@media (max-width: 480px) {
  .api-input-wide-card {
    padding: 1rem;
  }
  
  .wide-card-title h2 {
    font-size: 1.25rem;
  }
  
  .wide-card-title p {
    font-size: 0.8rem;
  }
  
  .wide-card-input {
    padding: 10px 12px;
    font-size: 14px;
  }
  
  .btn-query {
    padding: 10px 16px;
    font-size: 14px;
  }
}
</style>