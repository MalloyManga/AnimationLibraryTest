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
</script>

<template>
  <div>
    <h5 class="w-full flex justify-center my-6">Emoji Animation Test.</h5>
    <draggable v-model="items" item-key="emoji" tag="ul" class="grid grid-cols-5 gap-2 p-2 bg-gray-100 rounded-lg">
      <!-- 作用域插槽 父组件访问子组件数据 这里解构赋值了 -->
      <!-- {element, index} 这里 element 相当于 item in items 里的 item  -->
      <!-- draggable 已经封装了 v-for 故通过插槽参数拿 -->
      <template #item="{ element }">
        <li @click="removeItem(element)"
          class="cursor-move flex items-center justify-center text-2xl p-3 bg-white shadow rounded-lg hover:scale-105 transition-transform">
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
