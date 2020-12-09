<template>
  <div class="add-task-form">
    <form @submit.prevent="addTask">
      <input
        placeholder="Add new task..."
        v-model="taskTitle"
        class="input-task"
      />
      <br />
      <label
        class="title-label"
        :class="{ invalidStyle: displayInputTitleInvalited }"
        >title is required</label
      >
      <p class="des-title" style="marginRight: 220px ">Description</p>
      <textarea
        v-model="taskDescription"
        class="input-description"
        placeholder="Descrioption"
      ></textarea>
      <br />
      <label
        class="title-label"
        :class="{ invalidStyle: displayInputDesInvalited }"
        >description is required</label
      >
      <div class="select-area">
        <div class="input-date-area">
          <p>Due date</p>
          <input
            v-model="taskDueDate"
            type="date"
            class="date-select"
            id="date"
          />
          <br/>
          <label
        class="title-label"
        :class="{ invalidStyle: displayInputDateInvalited }"
        >due date is required</label
      >
        </div>
        <div class="input-priority-area">
          <p>priority</p>
          <select v-model="taskPriority">
            <option value="1">Not Important</option>
            <option value="2">Normal</option>
            <option value="3">Important</option>
          </select>
          <br />
          <label
            class="title-label"
            :class="{ invalidStyle: displayInputPriorityInvalited }"
            >priority is required</label
          >
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
      taskTitle: "",
      taskDescription: "",
      taskDueDate: "",
      taskPriority: "2",
      isDone: false,
      submitted: false,
      displayInputTitleInvalited: false,
      displayInputDesInvalited: false,
      displayInputDateInvalited: false,
      displayInputPriorityInvalited: false,
    };
  },

  methods: {
    addTask() {
      //todo: check validate form
        if(this.taskTitle.length < 1){
          this.displayInputTitleInvalited = true
        }
        else if(this.taskDescription.length < 1){
          this.displayInputDesInvalited = true
        }
        else if(this.taskDueDate == ""){
          this.displayInputDateInvalited = true
        }
        else if(this.taskPriority == ""){
          this.displayInputPriorityInvalited = true
        }
       else {
        const newTask = {
          id: uuidv4(),
          title: this.taskTitle,
          description: this.taskDescription,
          dueDate: this.taskDueDate,
          priority: this.taskPriority,
          status: this.isDone,
        };
        //send up to parent
        this.$emit("addTask", newTask);
        this.taskTitle = "";
        this.taskDescription = "";
        this.displayInputTitleInvalited = false;
        this.displayInputDesInvalited = false;
        this.displayInputDateInvalited = false;
        this.displayInputPriorityInvalited = false;
      }
    },
    //note: dont handle in real DOM
    // set default date is today
    getDate() {
      var today = new Date();
      this.taskDueDate =
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
  margin-top: 20px;
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

.title-label {
  color: red;
  font-size: 12px;
  float: left;
  margin-left: 20px;
  display: none;
}
.invalidStyle {
  display: block;
}
@media screen and (max-width: 1024px) {
  .add-task-form {
    height: 200px;
    max-width: 580px;
    border: 1px solid grey;
  }
  .input-task {
    width: 100%;
  }
  .input-description {
    height: 40px;
    width: 100%;
    margin-top: 20px;
  }
  .des-title {
    display: none;
  }
  .add-btn {
    width: 100px;
  }
  .select-area {
    justify-content: center;
  }
  .input-date-area {
    margin-right: 200px;
  }
}
</style>
