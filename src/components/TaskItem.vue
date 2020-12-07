<template>
  <div>
    <div class="item">
      <input
        class="input-status"
        type="checkbox"
        @change="handleDoneTask(todo.id)"
      />
      <p class="title" v-bind:class="{ done:todo.status }">{{ todo.title }}</p>
      <div class="item-action">
        <button class="upd" @click="handleUpdate(todo.id)">Detail</button>
        <button class="del" @click="handleDelete(todo.id)">Remove</button>
      </div>
    </div>
    <div class="update-form" v-bind:class="{ isUpdate: update }">
      <form @submit.prevent="updateTask">
        <input
          placeholder="Add new task..."
          class="input-task"
          v-bind:value="todo.title"
          @change="updateTaskTitle"
        />
        <p class="des-title">Description</p>
        <textarea
          class="input-description"
          placeholder="Descrioption"
          v-bind:value="todo.description"
          @change="updateTaskDes"
        ></textarea>
        <div class="select-area">
          <div class="input-date-area">
            <p style="fontSize:14px">Due date</p>
            <input type="date" class="date-select" 
            id="datePicker" v-bind:value="todo.dueDate" @change="updateTaskDueDate"  />
          </div>
          <div class="input-priority-area">
            <p style="fontSize:14px">Priority</p>
            <select v-bind:value="todo.priority" @change="updateTaskPriority" >
              <option value="1">Not Important</option>
              <option value="2" >Normal</option>
              <option value="3">Important</option>
            </select>
          </div>
        </div>

        <button class="submit-btn">Update</button>
      </form>
    </div>
  </div>
</template>

<script>

export default {
  name: "TaskItem",
  props: ["todo"],
  methods: {
    handleDelete(id) {
      this.$emit("handleDelete", id);
    },
    handleUpdate(id) {
      this.$emit("handleUpdate", id);
      this.update = !this.update;
    },
    handleDoneTask(id) {
      this.$emit("handleDoneTask", id);
    },
    updateTaskTitle(e){
      this.TaskTitle = e.target.value
    },
    updateTaskDes(e){
      this.TaskDescription = e.target.value
    },
    updateTaskDueDate(e){
      this.TaskDueDate = e.target.value
    },
    updateTaskPriority(e){
      this.TaskPriority = e.target.value
    },
    updateTask(){
      const updateTask = {
        id: this.todo.id,
        title: this.TaskTitle,
        description: this.TaskDescription,
        dueDate: this.TaskDueDate,
        priority: this.TaskPriority,
        status: this.status
      };
      this.$emit('updateTask', updateTask);
      this.update = !this.update
    },
  },
  data() {
    return {
      update: true,
      TaskTitle: '',
      TaskDescription: '',
      TaskDueDate: '',
      TaskPriority: '', 
      status: false
    };
  },
};
</script>





<style scoped>
.item {
  border: 1px solid #ccc;
  display: flex;
  flex-direction: row;
  position: relative;
}
.title {
  margin-left: 20px;
  font-size: 16px;
}
.input-status {
  margin-top: 10px;
  margin-left: 15px;
  height: 25px;
  width: 25px;
}
.item-action {
  position: absolute;
  right: 5%;
  top: 30%;
}
.del {
  border: none;
  outline: none;
  background: #00bcd4;
  padding: 5px 10px;
  border-radius: 3px;
  color: white;
}
.upd {
  border: none;
  outline: none;
  background: #d9534f;
  margin-right: 20px;
  padding: 5px 10px;
  border-radius: 3px;
  color: white;
}
.update-form {
}
.isUpdate {
  display: none;
}
.done {
  text-decoration: line-through;
}
.input-task{
  width: 90%;
  height: 30px;
  margin-top: 20px;
  border: 1px solid #ccc;
  margin-right: 10px;
  outline: none;
}
.input-description{
  width: 90%;
  height: 50px;
  resize: none;
}
.des-title{
  font-size: 14px;
  float:left;
  margin-left: 20px;
}
.select-area{
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  width: 100%;
}
.submit-btn{
  width: 90%;
  background: #4caf50;
  color: white;
  margin-top: 10px;
  padding: 8px 0;
  border: none;
  border-radius: 3px;
  outline: none;
}
</style>
