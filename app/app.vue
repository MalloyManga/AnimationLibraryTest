<script setup>
import { ref } from 'vue'
const items = ref([
  "😏", "😐", "😑", "😒", "😕",
  "😀", "😂", "😍", "🥰", "😎",
  "😭", "😡", "🤔", "😴", "😱"
])
function removeItem(toRemove) {
  items.value = items.value.filter((item) => item !== toRemove)
}
</script>

<template>
  <div>
    <h5>Click emojis to remove them.</h5>
    <client-only>
      <ul v-auto-animate class="emoji-list">
        <li v-for="item in items" :key="item" @click="removeItem(item)" class="emoji-item">
          {{ item }}
        </li>
      </ul>

      <template #fallback>
        <!-- 简单的静态占位，避免布局跳动 -->
        <ul class="emoji-list">
          <li v-for="item in items" :key="item" class="emoji-item" @click="removeItem(item)">
            {{ item }}
          </li>
        </ul>
      </template>
    </client-only>
  </div>
</template>

<style scoped>
.emoji-list {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  padding: 0;
  margin: 0;
  list-style: none;
}

.emoji-item {
  background: #f5f5f5;
  border-radius: 12px;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  transition: background 0.2s;
  user-select: none;
}

.emoji-item:hover {
  background: #e0e0e0;
}
</style>