<script setup>
import { ref } from 'vue'
import draggable from 'vuedraggable'
const items = ref([
  "😏", "😐", "😑", "😒", "😕",
  "😀", "😂", "😍", "🥰", "😎",
  "😭", "😡", "🤔", "😴", "😱"
])
function removeItem(toRemove) {
  items.value = items.value.filter((item) => item !== toRemove)
}

function reorder() {
  items.value.sort(() => Math.random() - 0.5)
}
</script>

<template>
  <div>
    <h5 class="w-full flex justify-center my-6">Emoji Animation Test.</h5>
    
    
    <!-- --------------------------Motion----------------- -->
    <div>
      <button @click="reorder" class="m-4 px-4 py-2 bg-blue-500 text-white rounded">Reorder</button>
      <ul class="grid grid-cols-5 gap-4 p-4">
        <AnimatePresence>
          <Motion v-for="item in items" :key="item" tag="li" :initial="{ opacity: 0, scale: 0.5 }"
            :animate="{ opacity: 1, scale: 1 }" :exit="{ opacity: 0, scale: 0.5 }" layout @click="removeItem(item)"
            class="cursor-pointer flex items-center justify-center text-3xl p-3 bg-white shadow rounded-lg">
            {{ item }}
          </Motion>
        </AnimatePresence>
      </ul>
    </div>
    <!-- --------------------------------------------------- -->
    <draggable v-model="items" item-key="emoji" tag="ul" class="grid grid-cols-5 gap-2 p-2 bg-gray-100 rounded-lg">
      <!-- 作用域插槽 父组件访问子组件数据 这里解构赋值了 -->
      <!-- {element, index} 这里 element 相当于 item in items 里的 item  -->
      <!-- draggable 已经封装了 v-for 故通过插槽参数拿 -->
      <template #item="{ element }">
        <li @click="removeItem(element)"
          class="flex items-center justify-center text-2xl p-3 bg-white shadow rounded-lg hover:scale-105 transition-transform">
          {{ element }}
        </li>
      </template>
    </draggable>
    <!-- ------------------------------------------------- -->
    <client-only>
      <ul v-auto-animate class="flex flex-wrap gap-3 list-none px-2 m-0">
        <li v-for="item in items" :key="item" @click="removeItem(item)"
          class="border-2 border-[#5371ad] flex h-12 w-12 cursor-pointer select-none items-center justify-center rounded-xl bg-gray-100 text-3xl transition-colors duration-200 hover:bg-gray-200">
          {{ item }}
        </li>
      </ul>

      <template #fallback>
        <!-- 简单的静态占位，避免布局跳动 -->
        <ul class="flex flex-wrap gap-3 list-none px-2 m-0">
          <li v-for="item in items" :key="item" @click="removeItem(item)"
            class="border-2 border-[#5371ad] flex h-12 w-12 cursor-pointer select-none items-center justify-center rounded-xl bg-gray-100 text-3xl">
            {{ item }}
          </li>
        </ul>
      </template>
    </client-only>
  </div>
</template>


<style scoped>
/* 只需要一个 ghost class 给 vuedraggable 就够了 */
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
  border-radius: 0.5rem;
}
</style>