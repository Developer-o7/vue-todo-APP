<template>
<div v-if="todos.length !== 0">
  <div
  class="card flex pt-2"
  v-for="todo in todos"
  :key="todo.id"
  >
    <h3>{{todo.todo}}</h3>
    <button @click="$emit('deleted', todo.id)" class="btn danger">Delete</button>
  </div>
</div>
<div class="card center" v-else>
  <h4>There are not todo yet</h4>
  <button class="btn danger" @click="$emit('loadTodo')">Load Todo</button>
</div>
</template>

<script>
export default {
  name: 'todoList',
  props: ['todos'],
  emits: ['loadTodo', 'deleted']
}
</script>

<style scoped>
.flex {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
