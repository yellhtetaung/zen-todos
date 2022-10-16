<template>
  <div id="app" class="container">
    <notifications group="foo" class="mt-3" />
    <AddTask />
    <div class="row justify-content-center mt-3">
      <TodoItem
        v-for="(task, index) in tasks"
        :key="index"
        :task="task"
        :index="index"
      />
    </div>
  </div>
</template>

<script>
import AddTask from "./components/AddTask.vue";
import TodoItem from "./components/TodoItem.vue";
import axios from "axios";
export default {
  name: "App",
  components: { AddTask, TodoItem },
  data() {
    return {
      tasks: [],
    };
  },
  async created() {
    await axios.get("http://localhost:3000/tasks").then((res) => {
      this.tasks = res.data;
    });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
