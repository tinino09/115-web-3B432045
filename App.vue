<script setup>
import { ref, computed } from 'vue'
import BaseButton from './components/BaseButton.vue'

// 三筆不同的模擬資料（文字取自網站原本的按鈕）
const buttons = ref([
  { id: 1, title: '探索工藝',   content: '認識木藝、陶藝、編織', variant: 'primary', size: 'big', href: '#craft' },
  { id: 2, title: '加入購物車', content: '把這件器物放進購物車', variant: 'primary', size: 'sm' },
  { id: 3, title: '重新作答',   content: '回到第一題重新選擇',   variant: 'link',    size: 'md' },
])

const lastClick = ref('尚未點擊')
function onClick(b) { lastClick.value = b.title }

// v-model + computed：勾選後，結帳按鈕才能按，文字也跟著換
const hasItems = ref(false)
const checkoutText = computed(() => (hasItems.value ? '結帳' : '購物車是空的'))
</script>

<template>
  <main class="page">
    <h1>BaseButton 元件示範</h1>
    <p class="lead">最近點擊：{{ lastClick }}</p>

    <h2>1. 三筆不同的資料（不同樣式、大小；滑鼠停在按鈕上會顯示 content）</h2>
    <div class="row">
      <BaseButton
        v-for="b in buttons" :key="b.id"
        :title="b.title" :content="b.content"
        :variant="b.variant" :size="b.size" :href="b.href"
        @click="onClick(b)"
      />
    </div>

    <h2>2. 三種大小</h2>
    <div class="row">
      <BaseButton size="sm" title="小按鈕" />
      <BaseButton size="md" title="中按鈕" />
      <BaseButton size="big" title="大按鈕" />
    </div>

    <h2>3. disabled 與 slot</h2>
    <label class="pick"><input type="checkbox" v-model="hasItems"> 購物車有商品</label>
    <div class="row">
      <BaseButton :title="checkoutText" :disabled="!hasItems" />
      <!-- 不傳 title，改用 slot 放自訂內容 -->
      <BaseButton size="sm">♥ 收藏</BaseButton>
    </div>
  </main>
</template>

<style>
/* 網站共用的色彩變數 */
:root { --ink: #33293a; --mu: #6b6275; --ln: #ecdbe2; --sf: #fff; --ac: #d63a60; --on: #fff; }
@media (prefers-color-scheme: dark) {
  :root { --ink: #f4eef7; --mu: #bcb0c6; --ln: #43384e; --sf: #262030; --ac: #ff8aa6; --on: #2a1620; }
}
body { margin: 0; background: var(--sf); color: var(--ink); font: 16px/1.75 'Noto Sans TC', 'Microsoft JhengHei', sans-serif; }
</style>

<style scoped>
.page { max-width: 800px; margin: 0 auto; padding: 32px 20px 64px; }
h1 { margin: 0 0 .2em; }
h2 { margin: 2em 0 .6em; font-size: 1.2rem; }
.lead { color: var(--mu); }
.row { display: flex; flex-wrap: wrap; align-items: center; gap: 16px; }
.pick { display: block; margin-bottom: 12px; }
</style>
