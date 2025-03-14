<template>
  <div>
    <div class="header">
      <h1>Todo List</h1>
    </div>
    <div class="input-container">
      <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new task" />
      <button @click="addTodo">提交</button>
    </div>
    <div class="todo-container">
      <div class="todo-section">
        <h2>正在进行</h2>
        <ul>
          <li v-for="todo in activeTodos" :key="todo.id">
            <input type="text" v-model="todo.text" @blur="updateTodo(todo)" />
            <button @click="completeTodo(todo)">完成</button>
            <button @click="removeTodo(todo.id)">删除</button>
          </li>
        </ul>
      </div>
      <div class="todo-section">
        <h2>已经完成</h2>
        <ul>
          <li v-for="completedTodo in completedTodos" :key="completedTodo.id">
            <input type="text" v-model="completedTodo.text" @blur="updateCompletedTodo(completedTodo)" />
            <button @click="removeTodo(completedTodo.id)">删除</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [], // 存储正在进行的任务
      completedTodos: [], // 存储已完成的任务
      nextId: 1, // 任务 ID
    };
  },
  computed: {
    activeTodos() {
      return this.todos; // 所有正在进行的任务
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() === '') return; // 不允许添加空任务
      this.todos.push({ id: this.nextId++, text: this.newTodo, completed: false });
      this.newTodo = ''; // 清空输入框
    },
    removeTodo(id) {
      // 从 todos 和 completedTodos 中删除任务
      this.todos = this.todos.filter(todo => todo.id !== id);
      this.completedTodos = this.completedTodos.filter(todo => todo.id !== id);
    },
    updateTodo(updatedTodo) {
      const index = this.todos.findIndex(todo => todo.id === updatedTodo.id);
      if (index !== -1) {
        this.todos[index].text = updatedTodo.text; // 更新任务内容
      }
    },
    updateCompletedTodo(updatedTodo) {
      const index = this.completedTodos.findIndex(todo => todo.id === updatedTodo.id);
      if (index !== -1) {
        this.completedTodos[index].text = updatedTodo.text; // 更新已完成任务的内容
      }
    },
    completeTodo(todo) {
      // 将任务标记为已完成，并添加到 completedTodos 中
      const index = this.todos.findIndex(t => t.id === todo.id);
      if (index !== -1) {
        this.completedTodos.push({ ...this.todos[index], completed: true });
        this.todos.splice(index, 1); // 从正在进行的任务中移除
      }
    },
  }
};
</script>

<style scoped>
.header {
  background: linear-gradient(to right, #007bff, #00bfff);
  padding: 20px;
  text-align: center;
  color: white;
}

.input-container {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}

input[type="text"] {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-right: 10px;
  width: 300px; /* 固定输入框宽度 */
}

button {
  padding: 10px 15px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #218838;
}

.todo-container {
  display: flex;
  justify-content: space-between;
  padding: 0 20px;
}

.todo-section {
  width: 48%; /* 每个部分占据几乎整个容器的一半 */
}

h2 {
  color: #333;
  border-bottom: 2px solid #007bff;
  padding-bottom: 10px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: white;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 4px;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
}

li input[type="text"] {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-right: 10px;
}
</style>