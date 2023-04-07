<template>
  <main>
    <h1>TUDO LIST</h1>
    <p>Create a list of tasks</p>

    <div class="create-new">
      <input
        type="text"
        v-model="newTask"
        placeholder="Add a new task"
        @keypress.enter="addTask"
      />

      <input
        type="text"
        v-model="newDiscription"
        placeholder="Add a new desc "
        @keypress.enter="addTask"
      />

      <button @click="addTask">Add</button>

      <button @click="GetTask">Get</button>
    </div>

    <div class="tasks">
      <div
        v-for="(task, i) in $store.state.tasks"
        :key="i"
        :class="`task ${!task.status ? 'is-complete' : ''}`"
      >
        <div class="content">{{ task.todo_name }}</div>
        <div class="buttons">
          <button>
            {{ task.status ? "Undo" : "Done" }}
          </button>
          <button @click="DeleteTask(task._id)" class="delete">Delete</button>
        </div>
      </div>
    </div>
  </main>
</template>

<script>

import axios from "axios";
export default {
  data() {
    return {
      newTask: "",
      newDiscription: "",
    };
  },
  mounted() {
    this.GetTask();
  },
  methods: {
    async addTask() {
      if (this.newTask && this.newDiscription) {
        let a = await axios.post("http://localhost:9090/list/addlist", {
          UserID: 1,
          todo_name: this.newTask,
          status: 1,
          describtion: this.newDiscription,
        });

        this.GetTask();
      }
    },
    async GetTask() {
      let a = await axios.get("http://localhost:9090/list/get-list");
      console.log(a.data.data);
      this.$store.commit("ADD_TASK", a.data.data);
    },

    async DeleteTask(id) {
      let a = await axios.delete(
        "http://localhost:9090/list/delete-list/" + id
      );
      this.GetTask();
    },
  },
};
</script>
