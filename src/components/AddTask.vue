<template>
  <div class="row justify-content-center">
    <h1 class="text-uppercase">Todo List</h1>
    <div class="col-lg-8 col-md-10 mt-5 text-start text-light">
      <label for="newtask">New Task</label>
      <input
        type="text"
        class="form-control mt-3 py-2 ps-3"
        placeholder="Enter New Task"
        v-model="task"
      />
      <button type="button" class="btn btn-warning mt-3" @click="addTask">
        Add Task
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "AddTask",
  data() {
    return {
      task: "",
    };
  },
  methods: {
    async addTask() {
      if (this.task === "") {
        return alert("Task field is required");
      } else {
        await axios.post("http://localhost:3000/tasks", {
          item: this.task,
          done: false,
        });
        this.task = "";
        this.$notify({
          group: "foo",
          title: "Created",
          text: "Your task has been created",
        });
        window.location.reload();
      }
    },
  },
};
</script>
