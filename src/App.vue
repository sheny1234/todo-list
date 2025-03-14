<template>
  <div id="app">
    <TodoList />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';

export default {
  components: {
    TodoList
  }
};
</script>

<style>
/* 你可以在这里添加全局样式 */
body {
  margin: 0;
  padding: 0;
}
</style>