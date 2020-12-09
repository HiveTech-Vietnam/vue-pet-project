<template>
  <div class="add-task-form">
    <form @submit.prevent="addTask">
      <input
        placeholder="Add new task..."
        v-model="TaskTitle"
        class="input-task"
      />
      <br/>
      <label class="title-label" :class="{invalidStyle: displayInputInvalited}">title is required</label>
      <p class="des-title" style="marginRight: 220px ">Description</p>
      <textarea
        v-model="TaskDescription"
        class="input-description"
        placeholder="Descrioption"
      ></textarea> <br/>
      <label class="title-label" :class="{invalidStyle: displayInputInvalited}">title is required</label>
      <div class="select-area">
        <div class="input-date-area">
          <p>Due date</p>
          <input
            v-model="TaskDueDate"
            type="date"
            class="date-select"
            id="date"
          />
        </div>
        <div class="input-priority-area">
          <p>priority</p>
          <select v-model="TaskPriority">
            <option value="1">Not Important</option>
            <option value="2" >Normal</option>
            <option value="3">Important</option>
          </select>
        </div>
      </div>

      <button class="add-btn">Add</button>
    </form>
  </div>
</template>

<script>
import { v4 as uuidv4 } from "uuid";

export default {
  name: "AddTask",
  mounted() {
    this.getDate();
  },
  data() {
    return {
      TaskTitle: "",
      TaskDescription: "",
      TaskDueDate: "",
      TaskPriority: "2",
      isDone: false,
      submitted: false,
      displayInputInvalited: false,
    };
  },

  methods: {
    addTask() {
      if (this.TaskTitle.length < 1 || this.TaskDescription.length < 1) {
        this.displayInputInvalited = true
      } else {
        const newTask = {
          id: uuidv4(),
          title: this.TaskTitle,
          description: this.TaskDescription,
          dueDate: this.TaskDueDate,
          priority: this.TaskPriority,
          status: this.isDone,
        };
        //send up to parent
        this.$emit("addTask", newTask);
        this.TaskTitle = "";
        this.TaskDescription = "";
        this.TaskDueDate = "";
        this.TaskPriority = "";
        this.displayInputInvalited = false
      }
    },

    // set default date is today
    getDate() {
      var today = new Date();

      document.getElementById("date").value =
        today.getFullYear() +
        "-" +
        ("0" + (today.getMonth() + 1)).slice(-2) +
        "-" +
        ("0" + today.getDate()).slice(-2);
    },
  },
};
</script>

<style scoped>
.add-task-form {
  border: 2px solid grey;
  border-right: none;
  padding: 50px;
  height: 579px;
}
.input-task {
  width: 300px;
  outline: none;
  height: 30px;
}
.input-task::placeholder {
  padding-left: 20px;
}
.input-description {
  width: 300px;
  resize: none;
  outline: none;
  height: 50px;
}
.input-description::placeholder {
  padding-left: 30px;
  padding-top: 10px;
}
.add-btn {
  width: 300px;
}
.select-area {
  display: flex;
  flex-direction: row;
}
.input-date-area {
  margin-left: 20px;
  margin-right: 50px;
}
.input-date-area p {
  margin-left: -70px;
}
.input-date-area input {
  height: 30px;
  outline: none;
}
.input-priority-area {
}
.input-priority-area select {
  height: 30px;
  outline: none;
}

.title-label{
  color: red;
  font-size: 12px;
  float: left;
  margin-left: 20px;
  display: none;  
}
.invalidStyle{
  display: block;
}
@media screen and (max-width: 1024px){
  .add-task-form{
    height: 200px;
    max-width: 580px;
    border: 1px solid grey;
  }
  .input-task{
    width: 100%;
  }
  .input-description{
    height: 40px;
    width: 100%;
    margin-top: 20px;
  }
  .des-title{
    display:none;
  }
  .add-btn{
    width:100px;
  }
  .select-area{
    justify-content: center;
  }
  .input-date-area{
    margin-right: 200px;
  }
}
</style>
