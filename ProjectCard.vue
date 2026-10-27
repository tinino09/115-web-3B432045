<script setup>
import { computed } from 'vue'
import CraftSwatch from './CraftSwatch.vue'

// props：由父元件傳進來的資料
const props = defineProps({
  craft:    { type: String, required: true, validator: v => ['w', 'p', 'k'].includes(v) }, // w 木藝／p 陶藝／k 編織
  title:    { type: String, required: true },          // 標題（商品名稱）
  content:  { type: String, default: '' },             // 內容（簡短說明）
  price:    { type: Number, default: null },           // 價格，沒傳就不顯示
  favorite: { type: Boolean, default: false },         // 是否已收藏（由父元件管理）
})

// emits：卡片自己不改資料，只通知父元件「使用者做了什麼」
const emit = defineEmits(['open', 'toggle-favorite', 'add-to-cart'])

const LABELS = { w: '木藝', p: '陶藝', k: '編織' }
const craftLabel = computed(() => LABELS[props.craft])
const priceText = computed(() => (props.price === null ? '' : 'NT$' + props.price.toLocaleString()))
</script>

<template>
  <article class="card" :data-c="craft">
    <!-- 點圖片：查看材質與工法 -->
    <button class="im" type="button" :aria-label="`查看${title}的材質與工法`" @click="emit('open')">
      <CraftSwatch :craft="craft" shape="square" />
    </button>

    <div class="cb">
      <span class="tag" :data-c="craft">{{ craftLabel }}</span>
      <h3>{{ title }}</h3>
      <p v-if="content" class="desc">{{ content }}</p>
      <p v-if="priceText" class="pr">{{ priceText }}</p>

      <div class="ac">
        <button
          class="hb" type="button"
          :aria-pressed="favorite" :aria-label="`收藏${title}`"
          @click="emit('toggle-favorite')"
        >{{ favorite ? '♥' : '♡' }}</button>
        <button class="btn" type="button" @click="emit('add-to-cart')">加入購物車</button>
      </div>
    </div>
  </article>
</template>

<style scoped>
.card { background: var(--sf, #fff); border: 3px solid var(--ln, #ecdbe2); border-radius: 26px; overflow: hidden; }
.card[data-c='w'] { border-color: var(--w, #f59a3c); }
.card[data-c='p'] { border-color: var(--p, #3fa9e0); }
.card[data-c='k'] { border-color: var(--k, #3dbb86); }

.im { display: block; width: 100%; padding: 0; border: 0; background: none; cursor: zoom-in; }
.im :deep(.sw) { border-radius: 0; }   /* 卡片上方的色塊貼齊邊框，不要圓角 */

.cb { padding: 12px 14px 14px; }
.tag { display: inline-block; padding: .05em .8em; border-radius: 99px; font-size: .8rem; font-weight: 700; color: #33293a; background: var(--w, #f59a3c); }
.tag[data-c='p'] { background: var(--p, #3fa9e0); }
.tag[data-c='k'] { background: var(--k, #3dbb86); }

h3 { margin: .4em 0 .2em; font-size: 1.15rem; line-height: 1.35; color: var(--ink, #33293a); }
.desc { margin: 0 0 .4em; color: var(--mu, #6b6275); font-size: .95rem; }
.pr { margin: 4px 0; font-weight: 700; color: var(--ink, #33293a); }

.ac { display: flex; justify-content: space-between; align-items: center; margin-top: 8px; }
.hb { font-size: 1.5rem; background: none; border: 0; color: var(--ac, #d63a60); cursor: pointer; }
.btn { background: var(--ac, #d63a60); color: var(--on, #fff); border: 0; border-radius: 999px; padding: .3em .9em; font: inherit; font-size: .9rem; font-weight: 700; cursor: pointer; box-shadow: 0 3px 0 rgba(0, 0, 0, .18); }
.btn:active { transform: translateY(2px); box-shadow: none; }
:focus-visible { outline: 3px solid var(--ac, #d63a60); outline-offset: 2px; }
</style>
