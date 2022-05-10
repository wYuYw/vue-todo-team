<template>
  <div class="todo--list">
    <ul v-for="todo in todos" :key="todo.id">
      <div v-if="todo.due === 0" :class="{completed: todo.done}" class="urgent">
        {{todo.content}}
        <div>
          D-day: {{todo.due}}
          <a @click="complete(todo)">
            <img src="../assets/icons/checkmark.svg" alt="done">
          </a>
          <a @click="del(todo)">
            <img src="../assets/icons/bin2.svg" alt="delete">
          </a>
          <!-- <button @click="complete(todo)">DONE</button> -->
        </div>
      </div>
      <div v-if="todo.due !== 0" :class="{completed: todo.done}" class="active">
        {{todo.content}}
        <div>
          D-day: {{todo.due}}
          <a @click="complete(todo)">
            <img src="../assets/icons/checkmark.svg" alt="done">
          </a>
          <a @click="del(todo)">
            <img src="../assets/icons/bin2.svg" alt="delete">
          </a>
          <!-- <button @click="complete(todo)">DONE</button> -->
        </div>
      </div>
    </ul>

  </div>
</template>

<script>

export default {
  name: 'TodoList',
  data() {
    return {
        todos: this.list,
    }
  },
  methods: {
    complete(todo) {
      todo.done = !todo.done;
      this.$emit('clickDone');
    },
    del(todo) {
      this.$emit('clickDelete', todo);
    }
  },
  props: ["list"]
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .active {
    width: 70%;
    font-size: 25px;
    font-weight: Bold;
    padding: 10px;
    display: flex;
    justify-content: space-between;
    box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.2);
  }
  .active:hover {
    color: #e3f0e1;
    background-color: #217d19;
    transform: scale(1.02);
    box-shadow: 0 0 5px #121212;
  }
  .urgent {
    width: 70%;
    font-size: 25px;
    font-weight: Bold;
    padding: 10px;
    display: flex;
    justify-content: space-between;
    background-color: #6ec066;
    box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.2);
  }
  .urgent:hover {
    color: #e3f0e1;
    background-color: #217d19;
    transform: scale(1.02);
    box-shadow: 0 0 5px #121212;
  }
  .completed {
    text-decoration: line-through;
    font-style: italic;
    background-color: #e3f0e1;
    box-shadow: #121212;
    color: #b3b3b3;
  }
  
</style>
