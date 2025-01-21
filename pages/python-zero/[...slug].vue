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
                <!-- 直接使用 catsQuery 数据进行调试 -->
                <div v-for="section in catsQuery" :key="section.title">
                  <h2 class="font-bold">{{ section.title }}</h2>
                  <ul>
                    <li v-for="item in section.items" :key="item._path">
                      <NuxtLink :to="item._path">{{ item.title }}</NuxtLink>
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

const route = useRoute()
const specialPages = ['/python-zero/preface', '/python-zero/preliminaries', '/python-zero/1python_basic', '/python-zero/2data_type', '/python-zero/3data_structure', '/python-zero/4functions', '/python-zero/5loops', '/python-zero/1numpyi', '/python-zero/2numpyii', '/python-zero/pandas', '/python-zero/probability', '/python-zero/cursor_tutorial']
const isSpecialPage = specialPages.includes(route.path)

const catsQuery = [
  {
    title: 'Start',
    items: [
      { _path: '/python-zero/preface', title: 'Preface' }
    ]
  },
  {
    title: 'Preparation',
    items: [
      { _path: '/python-zero/preliminaries', title: '1 Preliminaries' }
    ]
  },
  {
    title: 'Python Basics',
    items: [
      { _path: '/python-zero/1python_basic', title: '2.1 Python Language Basics, IPython, and Jupyter Notebooks' },
      { _path: '/python-zero/2data_type', title: '2.2 Data Types and Variables' },
      { _path: '/python-zero/3data_structure', title: '2.3 Data Structures' },
      { _path: '/python-zero/4functions', title: '2.4 Functions' },
      { _path: '/python-zero/5loops', title: '2.5 Loops' }
    ]
  },
  {
    title: 'Numpy',
    items: [
      { _path: '/python-zero/1numpyi', title: '3.1 Numpy for Python I' },
      { _path: '/python-zero/2numpyii', title: '3.2 Numpy for Python II' }
    ]
  },
  {
    title: 'Pandas',
    items: [
      { _path: '/python-zero/pandas', title: '4 Getting Started with pandas' }
    ]
  },
  {
    title: 'Probability and Statistics',
    items: [
      { _path: '/python-zero/probability', title: '5 Probability and Statistics' }
    ]
  },
  {
    title: 'AI',
    items: [
      { _path: '/python-zero/cursor_tutorial', title: 'Cursor Tutorial' }
    ]
  }
];
</script>
