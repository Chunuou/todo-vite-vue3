<script setup>
import { computed, reactive, ref } from 'vue'

const input = ref('')

const todos = reactive([])

const tab = ref('')

const tabs = ['全部', '待办', '已完成']

const filteredTodos = computed(() => {
  switch (tab.value) {
    case '全部':
      return todos

    case '待办':
      return todos.filter(i => !i.isChecked)

    case '已完成':
      return todos.filter(i => i.isChecked)

    default:
      return todos
  }
})

const add = () => {
  if (!input.value) return
  if (!input.value.trim()) return

  todos.push({
    name: input.value,
    time: new Date().toISOString(),
    isChecked: false
  })

  input.value = ''
}
</script>

<template>
  <input
    type="text"
    v-model="input"
    @keypress.enter.stop="add"
    placeholder="按下 enter 添加项目"
  />

  <div>
    <button v-for="item in tabs" :key="item" @click="tab = item">
      {{ item }}
    </button>
  </div>

  <ul>
    <li
      v-for="item in filteredTodos"
      :key="item.name"
      :class="item.isChecked ? 'checked' : 'todo'"
    >
      {{ item.name }}, {{ item.time }}
      <input
        type="checkbox"
        :checked="item.isChecked"
        @change="item.isChecked = !item.isChecked"
      />
    </li>
  </ul>
</template>

<style>
li::marker {
  display: none;
}
.todo {
  color: rgb(255, 187, 0);
}
.checked {
  color: #339933;
  text-decoration: line-through;
}
</style>
