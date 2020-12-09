<template>
  <div id="app">
    <div class="add-task-form">
      <AddTask
        @addTask="handleAddTask($event)"
        :class="{ toogleForm: isAdd }"
      />
    </div>
    <div class="main">
      <button @click="toogleAddForm" class="add-new-task">Add New Task</button>
      <h2>Todolist</h2>
      <div class="sort-task">
        <label>Sort</label> <br />
        <select v-model="sortQuery">
          <option value="0">All</option>
          <option value="1">Not inportant</option>
          <option value="2">Normal</option>
          <option value="3">Important</option>
        </select>
      </div>
      <br />
      <input
        v-model="searchQuery"
        placeholder="Search task..."
        class="search-task"
      />
      <TaskList
        v-on:DeleteTask="DeleteTask($event)"
        v-on:UpdateTask="handleUpdate($event)"
        v-on:handleDoneTask="handleClickedTask($event)"
        v-on:getDataUpdate="getDataUpdate($event)"
        v-bind:filterTask="filterTask"
      />
      <div id="bulk-action">
        <button class="done-all-btn " v-on:click="doneAll">Done</button>
        <button v-on:click="removeAll" class="remove-all-btn">
          Remove
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import AddTask from "./components/AddTask";
import TaskList from "./components/TaskList";
import _ from "lodash";

export default {
  name: "App",
  components: {
    AddTask,
    TaskList,
  },
  created() {
    this.saveToLocalData();
    this.todos = JSON.parse(localStorage.getItem("todos"));
  },
  beforeUpdate() {
    this.sortTask();
  },
  // note: declare data before methods
  data() {
    return {
      todos: [],
      dataUpdate: "",
      keySearch: "",
      taskUpdate: "",
      isAdd: true,
      searchQuery: "",
      sortQuery: "0",
      sortData: null,
      temporaryId: [],
    };
  },
  methods: {
    saveToLocalData() {
      let parsed = JSON.stringify(this.todos);
      localStorage.setItem("todos", parsed);
    },

    handleAddTask(task) {
      this.todos = [...this.todos, task];
      this.saveToLocalData();
      this.isAdd = true;
    },
    DeleteTask(id) {
      let index = this.todos.findIndex((task) => task.id === id);
      this.todos.splice(index, 1);
      this.saveToLocalData();
    },
    handleUpdate(id) {
      this.taskUpdate = this.todos.findIndex((task) => task.id === id);
      this.saveToLocalData();
    },

    getDataUpdate(data) {
      this.dataUpdate = data;
      this.todos.splice(this.taskUpdate, 1, this.dataUpdate);
      this.saveToLocalData();
    },

    toogleAddForm() {
      this.isAdd = !this.isAdd;
    },
    //sort task
    sortTask() {
      switch (this.sortQuery) {
        case "0":
          this.sortData = [...this.todos];
          break;
        case "1":
          this.sortData = this.todos.filter((task) => task.priority === "1");
          break;
        case "2":
          this.sortData = this.todos.filter((task) => task.priority === "2");
          break;
        case "3":
          this.sortData = this.todos.filter((task) => task.priority === "3");
          break;
      }
    },
    //todo: in bulk action, save id of task-clicked in temporaryId arr to handle when trigger each of bulk action
    handleClickedTask(id) {
      this.temporaryId = [...this.temporaryId, id];
    },
    removeAll() {
      for (let x = 0; x < this.temporaryId.length; x++) {
        this.todos = this.todos.filter(
          (task) => task.id != this.temporaryId[x]
        );
      }
      this.saveToLocalData();
    },

    doneAll() {
      for (let y = 0; y < this.temporaryId.length; y++) {
        this.todos.forEach((task) => {
          if (task.id === this.temporaryId[y]) {
            task.status = true;
          }
        });
      }
      this.saveToLocalData();
    },
  },
  computed: {
    filterTask() {
      console.log(this.todos);
      return _.filter(this.sortData, (task) =>
        task.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
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
  display: flex;
  flex-direction: row;
  justify-content: center;
  block-size: auto;
  position: relative;
}

.active {
  text-decoration: line-through;
}
.main {
  border: black solid 1px;
  width: auto;
  max-width: 600px;
  height: 600px;
  overflow: scroll;
  padding: 40px;
  position: relative;
}

::-webkit-scrollbar {
  width: 10px;
}
::-webkit-scrollbar-button {
  height: 20px;
}

.add-btn {
  background: #4caf50;
  border: none;
  outline: none;
  border-radius: 5px;
  padding: 10px;
  margin: 20px;
  color: white;
}
#bulk-action {
  position: fixed;
  bottom: 13px;
  justify-content: center;
  margin-bottom: 10px;
}
.done-all-btn {
  padding: 10px 25px;
  background: #d9534f;
  border-radius: 5px;
  outline: none;
  border: none;
  color: white;
  margin-right: 20px;
}
.remove-all-btn {
  padding: 10px 15px;
  background: #2196f3;
  border-radius: 5px;
  outline: none;
  border: none;
  color: white;
}
.add-new-task {
  background: #d9534f;
  border: none;
  outline: none;
  color: white;
  padding: 8px 15px;
  border-radius: 4px;
  display: none;
}
.sort-task {
  position: absolute;
  right: 10%;
  top: 4%;
}
.sort-task select {
  width: 100px;
  height: 30px;
  border-radius: 5px;
  background: #d9534f;
  color: white;
  outline: none;
  margin-top: 10px;
  border: none;
}
.search-task {
  width: 500px;
  height: 30px;
  padding-left: 50px;
  outline: none;
  margin-bottom: 10px;
}
@media screen and (max-width: 1024px) {
  .add-new-task {
    display: block;
  }
  #app {
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin-left: 40px;
  }

  .toogleForm {
    display: none;
  }
  #bulk-action {
    position: absolute;
    bottom: 0;
  }
}
</style>
