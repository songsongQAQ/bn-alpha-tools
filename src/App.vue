<script setup>
import { RouterView } from 'vue-router'
import { ref, onMounted, onUnmounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

// 屏幕尺寸检测
const isMobile = ref(false)
const isMenuOpen = ref(false)

// 菜单项配置
const menuItems = [
  { name: '首页', path: '/', icon: 'fas fa-home', gradient: 'from-blue-500 to-purple-600' },
  { name: '多地址管理', path: '/address-manager', icon: 'fas fa-address-book', gradient: 'from-indigo-500 to-purple-600' },
  { name: '空投预告', path: '/airdrop-preview', icon: 'fas fa-gift', gradient: 'from-purple-500 to-pink-600' },
  { name: 'Alpha积分规则', path: '/alpha-points-rules', icon: 'fas fa-coins', gradient: 'from-yellow-500 to-orange-600' },
  // { name: '使用帮助', path: '/help', icon: 'fas fa-question-circle', gradient: 'from-green-500 to-blue-600' },
  { name: '联系我们', path: '/contact', icon: 'fas fa-envelope', gradient: 'from-cyan-500 to-teal-600' },
  { name: '打赏猪脚饭', path: '/donation', icon: 'fas fa-utensils', gradient: 'from-yellow-500 to-red-600' }
]

// 检测屏幕尺寸
const checkScreenSize = () => {
  isMobile.value = window.innerWidth < 768
}

// 导航到指定路径
const navigateTo = (path) => {
  router.push(path)
  isMenuOpen.value = false
}

// 切换菜单
const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}

// 关闭菜单
const closeMenu = () => {
  isMenuOpen.value = false
}

// 判断当前路由是否激活
const isActiveRoute = (path) => {
  if (path === '/') {
    return route.path === '/'
  }
  return route.path.startsWith(path)
}

onMounted(() => {
  checkScreenSize()
  window.addEventListener('resize', checkScreenSize)
})

onUnmounted(() => {
  window.removeEventListener('resize', checkScreenSize)
})
</script>

<template>
  <div id="app" class="min-h-screen bg-gradient-to-br from-blue-400 via-purple-500 to-blue-600">
    <!-- 全局背景装饰 -->
    <div class="fixed inset-0 overflow-hidden pointer-events-none">
      <div class="absolute -top-40 -right-40 w-80 h-80 bg-gradient-to-br from-blue-300/30 to-purple-400/30 rounded-full blur-3xl animate-pulse"></div>
      <div class="absolute -bottom-40 -left-40 w-96 h-96 bg-gradient-to-br from-purple-300/30 to-blue-400/30 rounded-full blur-3xl"></div>
      <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] bg-gradient-to-br from-blue-400/20 to-purple-500/20 rounded-full blur-3xl"></div>
    </div>

    <!-- PC端顶部导航栏 -->
    <nav v-if="!isMobile" class="relative z-50 backdrop-blur-xl">
      <!-- 导航背景融合层 -->
      <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/5 to-transparent"></div>

      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
        <div class="flex justify-between h-16">
          <div class="flex items-center">
            <!-- Logo区域 -->
            <div class="flex items-center mr-8 group">
              <div class="w-10 h-10 bg-white/20 backdrop-blur-lg rounded-xl flex items-center justify-center mr-3 transform group-hover:scale-110 transition-all duration-300 shadow-xl border border-white/30">
                <i class="fas fa-cube text-white text-lg drop-shadow-lg"></i>
              </div>
              <h1 class="text-xl font-bold text-white drop-shadow-lg">
                Binance Alpha tool
              </h1>
            </div>

            <!-- Tab 导航 -->
            <div class="flex space-x-3">
              <button
                v-for="item in menuItems"
                :key="item.path"
                @click="navigateTo(item.path)"
                :class="[
                  'relative inline-flex items-center px-6 py-3 rounded-2xl text-sm font-semibold transition-all duration-300 transform hover:scale-105 backdrop-blur-lg border',
                  isActiveRoute(item.path)
                    ? 'text-white shadow-2xl border-white/40 bg-white/20'
                    : 'text-white/90 hover:text-white hover:bg-white/10 border-white/20 hover:border-white/30 hover:shadow-xl'
                ]"
                :style="isActiveRoute(item.path) ? `background: linear-gradient(135deg, ${item.gradient.replace('from-', '').replace('to-', ', ')}, rgba(255,255,255,0.1));` : ''"
              >
                <!-- 活跃状态的内发光效果 -->
                <div
                  v-if="isActiveRoute(item.path)"
                  :class="`absolute inset-0 rounded-2xl bg-gradient-to-r ${item.gradient} opacity-40 blur-lg scale-110`"
                ></div>

                <!-- 非活跃状态的背景光晕 -->
                <div
                  v-else
                  class="absolute inset-0 rounded-2xl bg-white/5 opacity-0 group-hover:opacity-100 transition-all duration-300"
                ></div>

                <i :class="[item.icon, 'mr-2 relative z-10 text-base drop-shadow-sm']"></i>
                <span class="relative z-10 font-medium drop-shadow-sm">{{ item.name }}</span>

                <!-- 活跃状态底部指示器 -->
                <div
                  v-if="isActiveRoute(item.path)"
                  class="absolute -bottom-1 left-1/2 transform -translate-x-1/2 w-3 h-3 bg-white rounded-full shadow-lg animate-pulse opacity-90"
                ></div>

                <!-- 悬停时的顶部光线 -->
                <div
                  v-if="!isActiveRoute(item.path)"
                  class="absolute -top-px left-4 right-4 h-px bg-gradient-to-r from-transparent via-white/60 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300"
                ></div>
              </button>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <!-- 移动端顶部导航栏 -->
    <nav v-if="isMobile" class="relative z-50 backdrop-blur-xl">
      <div class="absolute inset-0 bg-white/10"></div>
      <div class="px-4 h-16 flex items-center justify-between relative z-10">
        <!-- 菜单按钮 -->
        <button
          @click="toggleMenu"
          class="relative p-3 rounded-xl bg-white/20 backdrop-blur-lg text-white shadow-xl border border-white/30 transform transition-all duration-300 hover:scale-110 active:scale-95"
        >
          <div class="absolute inset-0 rounded-xl bg-white/10 opacity-0 hover:opacity-100 transition-opacity duration-300 blur-sm"></div>
          <i :class="[
            'text-lg relative z-10 transition-transform duration-300 drop-shadow-lg',
            isMenuOpen ? 'fas fa-times rotate-180' : 'fas fa-bars'
          ]"></i>
        </button>

        <!-- 标题 -->
        <div class="flex items-center">
          <div class="w-6 h-6 bg-white/20 backdrop-blur-lg rounded-lg flex items-center justify-center mr-2 border border-white/30">
            <i class="fas fa-cube text-white text-xs drop-shadow-lg"></i>
          </div>
          <h1 class="text-lg font-semibold text-white drop-shadow-lg">
            Binance Alpha tool
          </h1>
        </div>

        <!-- 当前页面指示器 -->
        <div class="w-8 h-8 rounded-lg bg-white/20 backdrop-blur-lg border border-white/30 flex items-center justify-center">
          <i :class="menuItems.find(item => isActiveRoute(item.path))?.icon || 'fas fa-home'" class="text-white text-sm drop-shadow-lg"></i>
        </div>
      </div>
    </nav>

    <!-- 移动端侧边抽屉菜单 -->
    <div
      v-if="isMobile"
      :class="[
        'fixed inset-0 z-50',
        isMenuOpen ? 'opacity-100 visible' : 'opacity-0 invisible'
      ]"
    >
      <!-- 静态遮罩层 -->
      <div
        @click="closeMenu"
        :class="[
          'absolute inset-0',
          isMenuOpen ? 'bg-black/30' : 'bg-black/0'
        ]"
      ></div>

      <!-- 抽屉内容 - 调小尺寸 -->
      <div
        :class="[
          'absolute left-0 top-0 h-full w-64 bg-white/95 backdrop-blur-xl shadow-2xl border-r border-gray-200/30',
          isMenuOpen ? 'translate-x-0' : '-translate-x-full'
        ]"
      >
        <!-- 抽屉头部 - 调小高度 -->
        <div class="h-16 bg-gradient-to-br from-blue-500 via-purple-600 to-blue-700 flex items-center justify-between px-4 relative overflow-hidden">
          <!-- 背景装饰 -->
          <div class="absolute inset-0 bg-gradient-to-br from-blue-400/20 to-purple-600/20"></div>
          <div class="absolute -top-3 -right-3 w-12 h-12 bg-white/10 rounded-full"></div>
          <div class="absolute -bottom-1 -left-1 w-8 h-8 bg-white/10 rounded-full"></div>

          <div class="flex items-center relative z-10">
            <div class="w-6 h-6 bg-white/20 backdrop-blur-sm rounded-lg flex items-center justify-center mr-2">
              <i class="fas fa-cube text-white text-sm"></i>
            </div>
            <h2 class="text-white font-bold text-base">导航菜单</h2>
          </div>

          <button
            @click="closeMenu"
            class="relative z-10 p-1.5 rounded-lg bg-white/20 backdrop-blur-sm text-white hover:bg-white/30"
          >
            <i class="fas fa-times text-base"></i>
          </button>
        </div>

        <!-- 菜单项 - 调小间距和大小 -->
        <div class="py-3 px-3 space-y-1">
          <button
            v-for="item in menuItems"
            :key="item.path"
            @click="navigateTo(item.path)"
            :class="[
              'w-full flex items-center px-4 py-3 rounded-lg text-left group relative overflow-hidden',
              isActiveRoute(item.path)
                ? 'text-white shadow-lg'
                : 'text-gray-700 hover:bg-gray-50 hover:shadow-md'
            ]"
            :style="isActiveRoute(item.path) ? `background: linear-gradient(135deg, ${item.gradient.replace('from-', '').replace('to-', ', ')});` : ''"
          >
            <!-- 背景效果 -->
            <div
              v-if="!isActiveRoute(item.path)"
              :class="`absolute inset-0 rounded-lg bg-gradient-to-r ${item.gradient} opacity-0 group-hover:opacity-10`"
            ></div>

            <!-- 活跃状态的光晕 -->
            <div
              v-if="isActiveRoute(item.path)"
              :class="`absolute inset-0 rounded-lg bg-gradient-to-r ${item.gradient} opacity-20`"
            ></div>

            <!-- 图标容器 - 调小尺寸 -->
            <div
              :class="[
                'w-8 h-8 rounded-lg flex items-center justify-center mr-3 relative z-10',
                isActiveRoute(item.path)
                  ? 'bg-white/20 backdrop-blur-sm'
                  : 'bg-gray-100 group-hover:bg-white'
              ]"
            >
              <i
                :class="[
                  item.icon,
                  'text-sm',
                  isActiveRoute(item.path) ? 'text-white' : 'text-gray-600 group-hover:text-gray-800'
                ]"
              ></i>
            </div>

            <!-- 文字内容 - 调小字体 -->
            <div class="relative z-10">
              <span class="font-medium text-sm">{{ item.name }}</span>
              <div
                v-if="isActiveRoute(item.path)"
                class="text-white/80 text-xs mt-0.5"
              >
                当前页面
              </div>
            </div>

            <!-- 右侧指示器 -->
            <div
              v-if="isActiveRoute(item.path)"
              class="ml-auto relative z-10"
            >
              <div class="w-1.5 h-1.5 bg-white rounded-full"></div>
            </div>

            <!-- 右侧箭头（非活跃状态） -->
            <div
              v-else
              class="ml-auto relative z-10 opacity-0 group-hover:opacity-100"
            >
              <i class="fas fa-chevron-right text-gray-400 text-xs"></i>
            </div>
          </button>
        </div>

        <!-- 底部装饰 - 调小尺寸 -->
        <div class="absolute bottom-3 left-3 right-3">
          <div class="bg-gradient-to-r from-gray-100 to-gray-50 rounded-lg p-3 border border-gray-200/50">
            <div class="flex items-center">
              <div class="w-6 h-6 bg-gradient-to-br from-blue-500 to-purple-600 rounded-lg flex items-center justify-center mr-2">
                <i class="fas fa-sparkles text-white text-xs"></i>
              </div>
              <div>
                <div class="text-xs font-medium text-gray-800">版本 v1.0.0</div>
                <div class="text-xs text-gray-500">BSC交易查询工具</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 主要内容区域 -->
    <main class="flex-1 relative z-10">
      <RouterView />
    </main>
  </div>
</template>

<style scoped>
#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  position: relative;
}

/* 确保页面内容不被固定导航遮挡 */
main {
  flex: 1;
}

/* 增强毛玻璃效果 */
.backdrop-blur-lg {
  backdrop-filter: blur(20px);
}

.backdrop-blur-xl {
  backdrop-filter: blur(30px);
}

.backdrop-blur-2xl {
  backdrop-filter: blur(40px);
}

.backdrop-blur-sm {
  backdrop-filter: blur(4px);
}

/* 增强模糊效果 */
.blur-2xl {
  filter: blur(40px);
}

.blur-3xl {
  filter: blur(60px);
}

/* 文字阴影效果 */
.drop-shadow-sm {
  filter: drop-shadow(0 1px 2px rgba(0, 0, 0, 0.2));
}

.drop-shadow-lg {
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.3));
}

/* 去除所有边界，实现完全融合 */
nav {
  position: sticky;
  top: 0;
  border: none !important;
  box-shadow: none !important;
  background: transparent;
}

/* PC端导航完全透明融合 */
nav.backdrop-blur-xl {
  background: rgba(255, 255, 255, 0.02);
  backdrop-filter: blur(30px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

/* 导航按钮增强 */
nav button {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(15px);
}

nav button:hover {
  border: 1px solid rgba(255, 255, 255, 0.3);
  background: rgba(255, 255, 255, 0.15);
  transform: translateY(-2px) scale(1.05);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}

/* Logo容器增强 */
.group div.bg-white\/20 {
  background: rgba(255, 255, 255, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.25);
  backdrop-filter: blur(20px);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

/* 文字颜色优化 */
.text-white {
  color: rgba(255, 255, 255, 0.95);
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
}

.text-white\/90 {
  color: rgba(255, 255, 255, 0.9);
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
}

/* 自定义动画 */
@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-10px); }
}

@keyframes pulse-glow {
  0%, 100% {
    box-shadow: 0 0 20px rgba(59, 130, 246, 0.3);
  }
  50% {
    box-shadow: 0 0 30px rgba(59, 130, 246, 0.6);
  }
}

/* 活跃状态的脉冲效果 */
.animate-pulse {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

/* 旋转动画 */
.rotate-180 {
  transform: rotate(180deg);
}

/* 阴影效果增强 */
.shadow-2xl {
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
}

/* 增强按钮激活状态 */
button[style*="background: linear-gradient"] {
  box-shadow:
    0 8px 25px rgba(0, 0, 0, 0.15),
    0 0 0 1px rgba(255, 255, 255, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.3);
  border: 1px solid rgba(255, 255, 255, 0.4) !important;
}

/* 全局背景动画 */
.fixed.inset-0 > div {
  animation: float 8s ease-in-out infinite;
}

.fixed.inset-0 > div:nth-child(1) {
  animation-delay: 0s;
}

.fixed.inset-0 > div:nth-child(2) {
  animation-delay: 2s;
}

.fixed.inset-0 > div:nth-child(3) {
  animation-delay: 4s;
}

/* 平滑过渡效果 */
* {
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
}

/* 移动端菜单按钮脉冲效果 */
.bg-white\/20:hover {
  animation: pulse-glow 2s ease-in-out infinite;
}

/* 响应式调整 */
@media (max-width: 640px) {
  .w-64 {
    width: 256px;
    max-width: 80vw;
  }
}

/* 移动端菜单简化样式 */
@media (max-width: 768px) {
  .space-y-1 > button {
    transition: background-color 0.2s ease;
  }

  .space-y-1 > button:hover {
    transform: none;
  }

  /* 简化过渡效果 */
  .fixed.inset-0 {
    transition: opacity 0.2s ease;
  }

  .absolute.left-0 {
    transition: transform 0.2s ease;
  }
}

/* 优化滚动条 */
::-webkit-scrollbar {
  width: 6px;
}

::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.3);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(255, 255, 255, 0.5);
}

/* 层次感优化 */
.relative.z-10 {
  position: relative;
  z-index: 10;
}

.relative.z-50 {
  position: relative;
  z-index: 50;
}

/* 去除边界线，完全融合 */
nav, nav * {
  border-image: none;
  border-style: solid;
}

/* 增强融合效果 */
nav::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(180deg, rgba(255,255,255,0.05) 0%, rgba(255,255,255,0.02) 50%, transparent 100%);
  pointer-events: none;
}

/* 移动端导航融合 */
@media (max-width: 768px) {
  nav.backdrop-blur-xl {
    background: rgba(255, 255, 255, 0.05);
    border-bottom: 1px solid rgba(255, 255, 255, 0.15);
  }
}
</style>

