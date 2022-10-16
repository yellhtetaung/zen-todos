<!--eslint-disable-->
<template>
  <div class="col-lg-8 col-md-10 mt-4">
    <div class="card p-3">
      <div class="row align-items-center">
        <div
          class="col-7 text-start"
          :class="{ taskCompleted: task.done }"
          @dblclick="editToggle = !editToggle"
        >
          <input
            type="text"
            class="form-control"
            v-if="editToggle"
            v-model="task.item"
            @keypress.enter="updateTask(index)"
          />
          <span v-else>{{ task.item }}</span>
        </div>
        <div class="col-5 text-end">
          <button class="btn btn-success mx-2" @click="completeTask(index)">
            <i class="material-icons mt-1">check</i>
          </button>
          <button class="btn btn-danger mx-2" @click="deleteTask(index)">
            <i class="material-icons mt-1">delete</i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "TodoItem",
  props: ["task", "index"],
  data() {
    return {
      editToggle: false,
      idx: "",
    };
  },
  methods: {
    async updateTask(index) {
      await axios.get("http://localhost:3000/tasks/").then((res) => {
        this.idx = res.data[index].id;
      });
      await axios.put("http://localhost:3000/tasks/" + this.idx, {
        item: this._props.task.item,
        done: false,
      });
      this.editToggle = false;
      this.$notify({
        group: "foo",
        title: "Edited",
        text: "Your task has been updated",
      });
      window.location.reload();
    },

    async completeTask(index) {
      await axios.get("http://localhost:3000/tasks/").then((res) => {
        this.idx = res.data[index].id;
      });
      await axios.put("http://localhost:3000/tasks/" + this.idx, {
        item: this._props.task.item,
        done: true,
      });
      this.$notify({
        group: "foo",
        title: "Completed",
        text: "Your task has been completed",
      });
      window.location.reload();
    },

    async deleteTask(index) {
      await axios.get("http://localhost:3000/tasks/").then((res) => {
        this.idx = res.data[index].id;
      });
      await axios.delete("http://localhost:3000/tasks/" + this.idx);
      window.location.reload();
      this.$notify({
        group: "foo",
        title: "Deleted",
        text: "Your task has been removed",
      });
    },
  },
};
</script>
