<template>
  <div class="todo--main">
    <h1>{{ msg }}</h1>
    <form v-on:submit="onSubmitForm" class="todo--input">
      <input v-model="todo" type="text" placeholder="Write your todo" class="todo--input-textbox">
      &nbsp;
      <input v-model="due" type="date" class="todo--input-datebox">
      <button class="todo--input-submit">GO!</button>
    </form>
    <hr />
    <TodoList :list="todos" v-on:clickDone="handleDoneClick" v-on:clickDelete="handleDeleteClick" class="todo--list" :key="todos"/>
    <TodoList :list="completedTodos" v-on:clickDone="handleDoneClick" v-on:clickDelete="handleDeleteClick" class="todo--list" :key="completedTodos"/>
  </div>
</template>

<script>
import TodoList from './TodoList.vue';

let todos = [];
let completedTodos = [];
function loadTaskList() {
  const loadedTodos = localStorage.getItem("todos");
  const loadedCompletedTodos = localStorage.getItem("completedTodos");
  if (loadedTodos !== null && loadedTodos !== []) {
    todos = JSON.parse(loadedTodos);
    todos.sort((a, b) => a.due - b.due);
  } else {
    console.log(loadedTodos);
    todos = [
  {
    id: 0,
    content: 'java rest api project',
    due: 0,
    done: false,
  },
  {
    id: 1,
    content: 'vue todo project',
    due: 2,
    done: false,
  },
  {
    id: 2,
    content: 'resume submit',
    due: 5,
    done: false,
  },
  ];
  }
  if (loadedCompletedTodos !== null && loadedCompletedTodos !== []) {
    completedTodos = JSON.parse(loadedCompletedTodos);
    completedTodos.sort((a, b) => a.due - b.due);
  } else {
    console.log(loadedCompletedTodos);
    completedTodos = [
      {
        id: 3,
        content: 'baekjooon 1212',
        due: 0,
        done: true,
      },
    ];
  }
}
loadTaskList();

localStorage.setItem("todos", JSON.stringify(todos));
localStorage.setItem("completedTodos", JSON.stringify(completedTodos));

export default {
  name: 'TodoInput',
  data() {
    return {
      todos: todos,
      completedTodos: completedTodos,
      todo: "",
      due: "",
    }
  },
  methods: {
    onSubmitForm(e) {
      e.preventDefault();
      if (this.todo && this.due) {
        let today = new Date();
        let dateInfo = this.due.split('-');
        let dday = new Date(dateInfo[0], dateInfo[1] - 1, dateInfo[2]);
        // console.log(dday)
        let gap = dday.getTime() - today.getTime();
        let result = Math.ceil(gap/(1000*60*60*24));
        // console.log(result)
        this.todos.push({id: Date.now(), content: this.todo, due: result, done: false});
        this.todos.sort((a, b) => a.due - b.due);
        localStorage.setItem("todos", JSON.stringify(this.todos));
        this.todo = "";
        this.due = "";
      } else {
        alert("Write your todo and select the due date");
      }
    },
    handleDoneClick() {
      let newTodos = this.todos.filter((item) => !item.done)
      let toBeCompleted = this.todos.filter((item) => item.done)
      
      let newCompletedTodos = this.completedTodos.filter((item) => item.done)
      let toBeOrigin = this.completedTodos.filter((item) => !item.done)

      this.completedTodos = [...newCompletedTodos, ...toBeCompleted];
      this.todos = [...newTodos, ...toBeOrigin];
      this.todos.sort((a, b) => a.due - b.due);
      this.completedTodos.sort((a, b) => a.due - b.due);

      localStorage.setItem("todos", JSON.stringify(this.todos));
      localStorage.setItem("completedTodos", JSON.stringify(this.completedTodos));

      // console.log(this.todos)
      // console.log(this.completedTodos)
    },
    handleDeleteClick(todo) {
      let newTodos = this.todos.filter((item) => item.id !== todo.id);
      let toBeCompleted = this.completedTodos.filter((item) => item.id !== todo.id);
      this.todos = [...newTodos];
      this.completedTodos = [...toBeCompleted];
      localStorage.setItem("todos", JSON.stringify(this.todos));
      localStorage.setItem("completedTodos", JSON.stringify(this.completedTodos));
    }
  },
  props: {
    msg: String
  },
  components: {
    TodoList
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  text-align: center;
  color: #2c3e50;
}
.todo--input {
  text-align: center;
}
.todo--input-textbox {
  width: 20%;
  border-radius: 50px;
  padding: 20px 30px;
  font-size: 25px;
  border: none;
  background: #eeeeee;
  transition: 0.2s;
  box-shadow: inset 0 0 5px #121212;
}
.todo--input-datebox {
  width: 20%;
  border-radius: 50px;
  padding: 20px 30px;
  font-size: 25px;
  border: none;
  background: #eeeeee;
  transition: 0.2s;
  box-shadow: inset 0 0 5px #121212;
}
.todo--input-submit {
    width: 50px;
    height: 50px;
    margin: 12px;
    border-radius: 50px;
    right: 0px;
    border: none;
    font-size: 15px;
    background-color: #30c222;
    color: #ffffff;
    transition: 0.2s all;
    box-shadow: 0 0 10px #121212;
}
.todo--input-submit:hover {
  background-color: #217d19;
  transform: scale(1.1);
  box-shadow: 0 0 5px #121212;
}
.todo--list {
  margin-left: 20%;
}
</style>
