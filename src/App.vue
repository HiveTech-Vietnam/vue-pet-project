<template>
  <div id="app">
    <div class="add-task-form">
      <AddTask v-on:addTask="handleAddTask($event)" :class="{toogleForm: isAdd}" />
    </div>
    <div class="main">
      <button @click="toogleAddForm" class="add-new-task" >Add New Task</button>
    
      <h2>Todolist</h2>

      <br />
      <Search
        v-on:querySearchKey="handleSearch($event)"
        style="marginBottom:15px"
      />
      <TaskList
        v-on:DeleteTask="DeleteTask($event)"
        v-on:UpdateTask="handleUpdate($event)"
        v-on:handleDoneTask="handleDoneTask($event)"
        v-on:getDataUpdate="getDataUpdate($event)"
        v-bind:todos="todos"
      />
      <div id="bulk-action">
        <button class="done-all-btn " v-on:click="doneAll">Done all</button>
        <button v-on:click="removeAll" class="remove-all-btn">
          Remove all
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import AddTask from "./components/AddTask";
import TaskList from "./components/TaskList";
import Search from "./components/Search";
export default {
  name: "App",
  components: {
    AddTask,
    TaskList,
    Search,
  },
  created () {
      this.localData();
      this.todos = JSON.parse(localStorage.getItem('todos'))
  },

  methods: {
    localData: function() {
      let parsed = JSON.stringify(this.todos);
      localStorage.setItem('todos', parsed)
    },
   
    handleAddTask(task) {
      this.todos = [...this.todos, task];
      this.localData();
      this.isAdd = true
    },
    DeleteTask: function(id) {
      let index = this.todos.findIndex((task) => task.id === id);
      this.todos.splice(index, 1);
      this.localData()
    },
    handleUpdate(id) {
      this.taskUpdate = this.todos.findIndex((task) => task.id === id);
      this.localData()
    },
    handleDoneTask(id) {
      let doneTask = this.todos.find((task) => task.id === id);
      doneTask.status = !doneTask.status;
      this.localData()
    },
    getDataUpdate(data) {
      this.dataUpdate = data;
      this.todos.splice(this.taskUpdate, 1, this.dataUpdate);
      this.localData()

    },
    handleSearch(key) {
      this.todos = this.todos.filter(
        (task) => task.title.indexOf(key.toLowerCase()) != -1
      );
    },
    removeAll() {
      this.todos = [];
      this.localData()

    },
    doneAll() {
      this.todos.forEach((todo) => (todo.status = true));
      this.localData()

    },
    toogleAddForm(){
      this.isAdd = !this.isAdd;
    }
  },
  data() {
    return {
      todos: [],
      dataUpdate: "",
      keySearch: "",
      taskUpdate: "",
      isAdd: true,
    };
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
  padding: 10px 15px;
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
.add-new-task{
  background: #d9534f;
  border: none;
  outline: none;
  color: white;
  padding: 8px 15px;
  border-radius: 4px;
  display: none;
}



@media screen and (max-width: 1024px){
  .add-new-task{
    display: block ;
  }
  #app{
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin-left: 40px;
  }

  .toogleForm{
    display: none;
  }
  #bulk-action{
    position: absolute;
    bottom: 0;
  }
}
</style>
