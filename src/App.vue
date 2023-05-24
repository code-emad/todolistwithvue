<script setup>
import {ref, onMounted, computed, watch} from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_catergory = ref(null)

const todos_ascending = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))
</script>

<template>
<div>
  Hello World
</div>
</template>

