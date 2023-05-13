<template>
  <div class="app">
    <h1>Список карточек</h1>
    <div v-if="todos.length === 0">Нет задач</div>
    <div v-else>
      <div v-for="todo in todos" :key="todo.id" class="card">
        <h2>{{ todo.title }}</h2>
        <p>{{ todo.description }}</p>
        <button @click="removeTodo(todo.id)">Удалить</button>
      </div>
    </div>
    <div class="add-todo">
      <h2>Добавить задачу</h2>
      <input v-model="newTodo.title" placeholder="Заголовок">
      <textarea v-model="newTodo.description" placeholder="Описание"></textarea>
      <button @click="createTodo">Сохранить</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [], // Локальный список задач
      newTodo: {
        title: '',
        description: '',
      },
    };
  },
  methods: {
    fetchTodos() {
      fetch('http://localhost:3100/api/todos')
          .then((response) => response.json())
          .then((data) => {
            this.todos = data;
          });
    },
    removeTodo(todoId) {
      fetch(`http://localhost:3100/api/todos/${todoId}`, {
        method: 'DELETE',
      })
          .then(() => {
            this.fetchTodos();
          })
          .catch((error) => {
            console.error(error);
          });
    },
    createTodo() {
      fetch('http://localhost:3100/api/todos', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(this.newTodo),
      })
          .then(() => {
            this.newTodo.title = '';
            this.newTodo.description = '';
            this.fetchTodos();
          })
          .catch((error) => {
            console.error(error);
          });
    },
  },
};
</script>
