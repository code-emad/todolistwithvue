<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const input_content = ref("");
const input_catergory = ref(null);

const todos_ascending = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

const addTodo = () => {
  if (input_content.value.trim() === "" || input_catergory.value === null) {
    return;
  }

  todos.value.push({
    content: input_content.value,
    category: input_catergory.value,
    createdAt: new Date().getTime(),
    done: false,
  });
};

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>

<template>
  <main class="app">
    <section class="create-todo">
      <h3>CREATE A TODO</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input
          type="text"
          placeholder="e.g. Buy some milk"
          v-model="input_content"
        />
        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              value="Work"
              v-model="input_catergory"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input
              type="radio"
              name="category"
              value="Personal"
              v-model="input_catergory"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
        <input type="submit" value="Add to list" />
      </form>
    </section>

    <section class="todo-list">
      <h3>List</h3>
      <div class="list"></div>

      <div
        v-for="todo in todos_asc"
        :class="`todo-item ${todo.done && 'done'}`"
      >
        <label>
          <input type="checkbox" v-model="todo.done" />
          <span :class="`bubble ${todo.category}`"></span>
        </label>
      </div>
    </section>
  </main>
</template>
