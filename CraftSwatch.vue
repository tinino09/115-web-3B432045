<script setup>
import { computed } from 'vue'

// props：由父元件傳進來的資料（單向，子元件不要直接修改）
const props = defineProps({
  craft:   { type: String, required: true, validator: v => ['w', 'p', 'k'].includes(v) }, // w 木藝／p 陶藝／k 編織
  title:   { type: String, default: '' },   // 標題
  content: { type: String, default: '' },   // 內容說明
  shape:   { type: String, default: 'square', validator: v => ['square', 'round', 'arch', 'portrait', 'compact'].includes(v) },
  layout:  { type: String, default: 'vertical', validator: v => ['vertical', 'horizontal'].includes(v) },
})

const CRAFTS = {
  w: { label: '木藝', emoji: '🪵' },
  p: { label: '陶藝', emoji: '🏺' },
  k: { label: '編織', emoji: '🧺' },
}
// computed：craft 一改變，圖示與文字就跟著換
const info = computed(() => CRAFTS[props.craft])
</script>

<template>
  <div class="craft" :class="`layout-${layout}`">
    <span class="sw" :class="`shape-${shape}`" :data-c="craft" role="img" :aria-label="`${info.label}示意圖`">
      <span class="emoji" aria-hidden="true">{{ info.emoji }}</span>
    </span>

    <!-- 有標題、內容或 slot 才顯示文字區 -->
    <div v-if="title || content || $slots.default" class="text">
      <h3 v-if="title">{{ title }}</h3>
      <p v-if="content">{{ content }}</p>
      <slot />   <!-- 額外內容（價格、按鈕、數量調整等）由父元件決定 -->
    </div>
  </div>
</template>

<style scoped>
.craft { display: grid; gap: 10px; }
.craft.layout-horizontal { display: flex; align-items: center; gap: 16px; }

/* 色塊：三種工藝各有紋理底色（沿用網站的 --w / --p / --k 變數） */
.sw { display: grid; place-items: center; --e: 64px; flex: none; }
.sw .emoji { font-size: var(--e); line-height: 1; }
.sw[data-c='w'] { background: radial-gradient(rgba(255,255,255,.5) 3px, transparent 4px) 0 0 / 22px 22px, var(--w, #f59a3c); }
.sw[data-c='p'] { background: radial-gradient(rgba(255,255,255,.5) 5px, transparent 6px) 0 0 / 30px 30px, var(--p, #3fa9e0); }
.sw[data-c='k'] { background: radial-gradient(rgba(255,255,255,.5) 2px, transparent 3px) 0 0 / 14px 14px, var(--k, #3dbb86); }

/* 形狀：對應網站六個使用位置 */
.shape-square   { width: 100%; aspect-ratio: 1; border-radius: 24px; }                     /* 商品卡 */
.shape-round    { width: 120px; aspect-ratio: 1; border-radius: 50%; --e: 44px; }         /* 工藝區塊標頭 */
.shape-arch     { width: 100%; aspect-ratio: 3 / 4; border-radius: 140px 140px 28px 28px; --e: 48px; } /* 首頁三入口 */
.shape-portrait { width: 100%; aspect-ratio: 4 / 5; border-radius: 24px; --e: 72px; }     /* 職人區、商品彈窗 */
.shape-compact  { width: 48px; aspect-ratio: 1; border-radius: 14px; --e: 22px; }         /* 購物車項目 */

/* 橫向排列時，滿版的形狀改用固定寬度 */
.layout-horizontal .shape-square   { width: 96px; }
.layout-horizontal .shape-arch     { width: 96px; }
.layout-horizontal .shape-portrait { width: 140px; }

.text { min-width: 0; }
h3 { margin: 0 0 .25em; font-size: 1.15rem; line-height: 1.35; color: var(--ink, #33293a); }
p  { margin: 0 0 .5em; color: var(--mu, #6b6275); max-width: 42em; }
</style>
