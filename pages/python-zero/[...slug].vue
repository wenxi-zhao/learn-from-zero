<template>
  <main class="min-h-[calc(100vh-var(--header-height))]">
    <ContentDoc v-slot="{ doc }">
      <div class="mx-auto px-4 sm:px-6 lg:px-8 max-w-screen-xl">
        <div class="flex flex-col lg:grid lg:grid-cols-10 lg:gap-8">
          <div class="lg:col-span-2">
            <!-- 这里开始是左侧的教程导航 -->
            <aside class="hidden overflow-y-auto lg:block lg:max-h-[calc(100vh-var(--header-height))] lg:sticky lg:top-[--header-height] py-8 lg:px-4 lg:-mx-4">
              <div class="sticky -top-[calc(100vh-var(--header-height))] -mt-8 z-[1]">
                <div class="h-8 -mx-4 px-4"></div>
                <div v-for="section in catsQuery" :key="section.title">
                  <div class="flex items-center cursor-pointer" @click="toggleSection(section)">
                    <h2 class="font-bold">{{ section.title }}</h2>
                    <span v-if="section.items && section.items.length > 1" 
                          class="ml-2 nav-arrow transform transition-transform" 
                          :class="{ 'rotate-90': section.isExpanded }">
                      <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
                        <path d="M10,17L15,12L10,7V17Z"/>
                      </svg>
                    </span>
                  </div>
                  <ul v-show="section.isExpanded">
                    <li v-for="item in section.items" 
                        :key="item._path" 
                        class="ml-4 my-2">
                      <NuxtLink :to="item._path" 
                                class="nav-link hover:text-blue-500 transition-colors duration-200"
                                :class="{ 'active-link': route.path === item._path }">
                        {{ item.title }}
                      </NuxtLink>
                    </li>
                  </ul>
                </div>
              </div>
            </aside>
          </div>
          
          <div class="lg:col-span-8">
            <div class="flex flex-col lg:grid lg:grid-cols-10 lg:gap-8">
              <div class="lg:col-span-8">
                <!-- 这里开始是正文部分 -->
                <article>
                  <div class="relative border-b border-gray-200 dark:border-gray-700 py-8">
                    <div class="flex flex-col lg:flex-row lg:items-center lg:justify-between">
                      <h1 class="text-3xl sm:text-4xl font-black tracking-tight">{{ doc.title }}</h1>
                    </div>
                    <p class="mt-6 leading-8">{{ doc.description ? doc.description : "" }}</p>
                  </div>
                  <div class="mt-8 pb-24 dark:prose-pre:!bg-gray-800/60 prose prose-primary dark:prose-invert max-w-none">
                    <ContentRenderer :value="doc" class="prose dark:prose-invert max-w-none"/>
                  </div>
                </article>

                <div v-if="!isSpecialPage" class="flex border-gray-200 dark:border-gray-800 w-full border-t border-dashed my-8">
                </div>
                
                <div v-if="!isSpecialPage" class="toolbar">
                  <div class="share-box" style="display:;">
                    分享到：
                    <ul class="share-list">
                      <li data-type="wechat" class="hover-item share-item has-qrcode-item">
                        <a href="javascript:void(0);" target="_self" class="dis-evt">
                          <i class="van-icon-share_wechat_default">
                          </i>
                        </a>
                        <div class="qrcode-box">
                          <p class="qrcode-tit">
                            微信扫一扫分享
                          </p>
                          <div id="wechat-qrcode" class="qrcode-ctnr">
                            <div class="van-qrcode">
                              <canvas width="132" height="132">
                              </canvas>
                            </div>
                          </div>
                        </div>
                      </li>
                    </ul>
                  </div>
                </div>
                
                <CommentArea v-if="!isSpecialPage" class="w-auto" />
              </div>
              <div class="lg:col-span-2 order-first lg:order-last">
                <!-- 这里开始是右侧的文章导航 -->
                <BodyNav :toc="doc.body.toc" />
              </div>

            </div>
          </div>
        </div>
      </div>
    </ContentDoc>
  </main>
</template>

<script setup>
import { useRoute } from 'vue-router'
import { ref, onMounted, watch } from 'vue'

const route = useRoute()
const specialPages = ['/python-zero/preface', '/python-zero/preliminaries', '/python-zero/1python_basic', '/python-zero/2data_type', '/python-zero/3data_structure', '/python-zero/4functions', '/python-zero/5loops', '/python-zero/1numpyi', '/python-zero/2numpyii', '/python-zero/pandas', '/python-zero/probability', '/python-zero/cursor_tutorial']
const isSpecialPage = specialPages.includes(route.path)

// 从 localStorage 获取保存的展开状态
const getSavedExpandState = () => {
  try {
    return JSON.parse(localStorage.getItem('navExpandState') || '{}')
  } catch {
    return {}
  }
}

const catsQuery = ref([
  {
    title: 'Start',
    isExpanded: true,
    items: [
      { _path: '/python-zero/preface', title: 'Preface' }
    ]
  },
  {
    title: 'Preparation',
    isExpanded: true,
    items: [
      { _path: '/python-zero/preliminaries', title: '1 Preliminaries' }
    ]
  },
  {
    title: 'Python Basics',
    isExpanded: true,
    items: [
      { _path: '/python-zero/1python_basic', title: '2.1 Python Language Basics' },
      { _path: '/python-zero/2data_type', title: '2.2 Data Types and Variables' },
      { _path: '/python-zero/3data_structure', title: '2.3 Data Structures' },
      { _path: '/python-zero/4functions', title: '2.4 Functions' },
      { _path: '/python-zero/5loops', title: '2.5 Loops' }
    ]
  },
  {
    title: 'Numpy',
    isExpanded: true,
    items: [
      { _path: '/python-zero/1numpyi', title: '3.1 Numpy for Python I' },
      { _path: '/python-zero/2numpyii', title: '3.2 Numpy for Python II' }
    ]
  },
  {
    title: 'Pandas',
    isExpanded: true,
    items: [
      { _path: '/python-zero/pandas', title: '4 Getting Started with pandas' }
    ]
  },
  {
    title: 'Probability and Statistics',
    isExpanded: true,
    items: [
      { _path: '/python-zero/probability', title: '5 Probability and Statistics' }
    ]
  },
  {
    title: 'AI',
    isExpanded: true,
    items: [
      { _path: '/python-zero/cursor_tutorial', title: 'Cursor Tutorial' }
    ]
  }
]);

// 修改初始化函数，确保没有保存状态时使用默认展开
onMounted(() => {
  const savedState = getSavedExpandState()
  if (Object.keys(savedState).length === 0) {
    // 如果没有保存的状态，不做任何改变，保持全部展开
    return;
  }
  // 如果有保存的状态，则使用保存的状态
  catsQuery.value.forEach(section => {
    if (savedState.hasOwnProperty(section.title)) {
      section.isExpanded = savedState[section.title]
    }
  })
})

// 切换展开/收缩状态并保存
const toggleSection = (section) => {
  section.isExpanded = !section.isExpanded
  
  // 保存所有部分的展开状态
  const expandState = {}
  catsQuery.value.forEach(s => {
    expandState[s.title] = s.isExpanded
  })
  localStorage.setItem('navExpandState', JSON.stringify(expandState))
}
</script>

<style>
/* 移除 scoped，使样式可以全局生效 */

.transform {
  transition: all 0.3s ease;
}

.nav-arrow {
  display: inline-flex;
  align-items: center;
  color: #64748b; /* 默认颜色 */
  font-size: 1.1em; /* 调整大小以匹配文字 */
}

.nav-link {
  position: relative;
  display: block;
  padding: 0.5rem 0;
}

.active-link {
  color: #3b82f6; /* 蓝色高亮 */
  font-weight: 600;
}

.active-link::before {
  content: '';
  position: absolute;
  left: -1rem;
  top: 50%;
  transform: translateY(-50%);
  width: 4px;
  height: 1.2rem;
  background-color: #3b82f6;
  border-radius: 2px;
}

/* 深色模式 */
:root.dark .nav-arrow {
  color: #94a3b8;
}

:root.dark .active-link {
  color: #60a5fa;
}

:root.dark .active-link::before {
  background-color: #60a5fa;
}

/* 隐藏滚动条但保持滚动功能 */
aside::-webkit-scrollbar,
.body-nav::-webkit-scrollbar {
  display: none !important; /* Chrome, Safari, Opera */
}

aside,
.body-nav {
  -ms-overflow-style: none !important;  /* IE and Edge */
  scrollbar-width: none !important;  /* Firefox */
}
</style>
