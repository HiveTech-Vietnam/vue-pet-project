<template>
  <div class="wrap-task-list">
    <div class="task-list" v-for="todo of filterTask" v-bind:key="todo.id">
      <TaskItem
        style="paddingBottom: 10px"
        v-bind:todo="todo"
        v-on:handleDelete="handleDelete($event)"
        v-on:handleUpdate="handleUpdate($event)"
        v-on:updateTask="getDataUpdate($event)"
        v-on:handleDoneTask="handleDoneTask($event)"
      />
    </div>
  </div>
</template>

<script>
import TaskItem from "./TaskItem";
export default {
  name: "TaskList",
  components: {
    TaskItem,
  },
  methods: {
    handleDelete(id) {
      this.$emit("DeleteTask", id);
    },
    handleUpdate(id) {
      this.$emit("UpdateTask", id);
    },
    handleDoneTask(id) {
      this.$emit("handleDoneTask", id);
    },
    getDataUpdate(updateTask) {
      this.$emit("getDataUpdate", updateTask);
    },
  },
  props: ["filterTask"],
};
</script>

<style scoped>
.wrap-task-list {
  overflow: auto;
}
</style>
