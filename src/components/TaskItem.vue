<template>
  <div>
    <div class="item">
      <input
        class="input-status"
        type="checkbox"
        @change="handleDoneTask(todo.id)"
      />
      <p class="title" v-bind:class="{ done: todo.status }">{{ todo.title }}</p>
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
          :value="todo.description"
          @change="updateTaskDes"
        ></textarea>
        <div class="select-area">
          <div class="input-date-area">
            <p style="fontSize:14px">Due date</p>
            <input
              type="date"
              class="date-select"
              id="datePicker"
              :value="todo.dueDate"
              @change="updateTaskDueDate"
            />
          </div>
          <div class="input-priority-area">
            <p style="fontSize:14px">Priority</p>
            <select :value="todo.priority" @change="updateTaskPriority">
              <option value="1">Not Important</option>
              <option value="2">Normal</option>
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
  created(){
    console.log(this.propsToStateToDo)
  },
  beforeUpdate(){
    this.renderTitlle(),
    this.renderDescription(),
    this.renderDueDate(),
    this.renderPriority()
  },
  // note declare data before method
  data() {
    return {
      update: true,
      taskTitle: "",
      taskDescription: "",
      taskDueDate: "",
      taskPriority: "",
      status: false,
      tempoTitle: '',
      tempoDescription:'',
      tempoDueDate:'',
      tempoPriority:''
    };
  },
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
      this.taskTitle = e.target.value;
      this.tempoTitle = e.target.value
    },
    updateTaskDes(e) {
      this.taskDescription = e.target.value;
    },
    updateTaskDueDate(e) {
      this.taskDueDate = e.target.value;
    },
    updateTaskPriority(e) {
      this.taskPriority = e.target.value;
    },
     renderTitlle(){
      if(this.tempoTitle == ''){
        this.taskTitle = this.todo.title
      }
      else{
        this.taskTitle = this.tempoTitle
      }
      return this.taskTitle
    },
    renderDescription(){
      if(this.tempoDescription == ''){
        this.taskDescription = this.todo.description
      }
      else{
        this.taskDescription = this.tempoDescription
      }
      return this.taskDescription
    },
    renderDueDate(){
      if(this.tempoDueDate == ''){
        this.taskDueDate = this.todo.dueDate
      }
      else{
        this.taskDueDate = this.tempoDueDate
      }
      return this.taskDueDate
    },
    renderPriority(){
      if(this.tempoPriority == ''){
        this.taskPriority = this.todo.priority
      }
      else{
        this.taskPriority = this.tempoPriority
      }
      return this.taskPriority
    },
    updateTask() {
      const updateTask = {
        id: this.todo.id,
        title: this.taskTitle,
        description: this.taskDescription,
        dueDate: this.taskDueDate,
        priority: this.taskPriority,
        status: this.status,
      };
      this.$emit("updateTask", updateTask);
      this.update = !this.update;
    },
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
.input-task {
  width: 90%;
  height: 30px;
  margin-top: 20px;
  border: 1px solid #ccc;
  margin-right: 10px;
  outline: none;
}
.input-description {
  width: 90%;
  height: 50px;
  resize: none;
}
.des-title {
  font-size: 14px;
  float: left;
  margin-left: 20px;
}
.select-area {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  width: 100%;
}
.submit-btn {
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
