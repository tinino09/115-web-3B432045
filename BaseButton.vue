<script setup>
import { computed } from 'vue'

// props：由父元件傳進來的資料
const props = defineProps({
  title:    { type: String, default: '' },    // 標題：按鈕上顯示的文字
  content:  { type: String, default: '' },    // 內容：補充說明，滑鼠停在按鈕上時會顯示
  variant:  { type: String, default: 'primary', validator: v => ['primary', 'link'].includes(v) },  // primary 實心按鈕／link 連結樣式
  size:     { type: String, default: 'md', validator: v => ['sm', 'md', 'big'].includes(v) },       // 大小
  type:     { type: String, default: 'button', validator: v => ['button', 'submit', 'reset'].includes(v) },
  href:     { type: String, default: '' },    // 有傳 href 就變成 <a> 連結（網站裡「探索工藝」這類按鈕）
  disabled: { type: Boolean, default: false },
})

// computed：有 href 用 <a>，沒有就用 <button>
const tag = computed(() => (props.href ? 'a' : 'button'))
</script>

<template>
  <component
    :is="tag"
    class="base-btn"
    :class="[`is-${variant}`, `size-${size}`]"
    :href="href || undefined"
    :type="href ? undefined : type"
    :disabled="href ? undefined : disabled"
    :title="content || undefined"
  >
    <!-- slot 有內容就用 slot，沒有就顯示 title -->
    <slot>{{ title }}</slot>
  </component>
</template>

<style scoped>
.base-btn { display: inline-block; font: inherit; font-weight: 700; text-decoration: none; cursor: pointer; border: 0; }

/* 實心按鈕（網站的 .btn） */
.is-primary { background: var(--ac, #d63a60); color: var(--on, #fff); border-radius: 999px; box-shadow: 0 3px 0 rgba(0, 0, 0, .18); transition: transform .15s; }
.is-primary:active { transform: translateY(2px); box-shadow: none; }
.is-primary.size-sm  { padding: .3em .8em; font-size: .9rem; }
.is-primary.size-md  { padding: .55em 1.1em; }
.is-primary.size-big { padding: .8em 1.8em; font-size: 1.1rem; }

/* 連結樣式按鈕（網站的 .lk） */
.is-link { background: none; color: var(--ac, #d63a60); text-decoration: underline; padding: 0; font-weight: 400; }
.is-link.size-sm  { font-size: .9rem; }
.is-link.size-big { font-size: 1.1rem; }

.base-btn:disabled { opacity: .4; cursor: not-allowed; }
.base-btn:focus-visible { outline: 3px solid var(--ac, #d63a60); outline-offset: 2px; }
</style>
