<template lang="pug">
  .todo
    todo-input(
      @addTodo="addTodo"
    )
    todo-list(
      :todos="todos"
      @checkTodo="checkTodo"
    )

</template>

<script>
import todoInput from "./todoInput";
import todoList from "./todoList";

import {eventBus} from './../main';

export default {
  components: {
    todoInput, todoList
  },
  props: {
    uid: String
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    addTodo(todo) {
      this.todos.push(todo);
      todo.id = this.todos.length - 1;

      this.$emit('pushTodos', this.todos);
    },
    removeTodo(todoId) {
      this.todos = this.todos.filter(item => item.id !== todoId);

      this.$emit('pushTodos', this.todos);
    },
    checkTodo(todo) {
      this.todos = this.todos.map(item => item.id === todo.id ? todo : item)

      this.$emit('pushTodos', this.todos);
    }
  },
  created() {
    this.todos = [];

    const takeTodos = firebase.database().ref('users/' + this.uid);
    takeTodos.on('value', (snapshot) => {
      if(snapshot.val()) {
        this.todos = snapshot.val().todos;
      } else {
        this.todos = [];
      }
    })
  },
  mounted() {
    eventBus.$on('removeTodo', todoId => {
      this.todos = this.todos.filter(item => item.id !== todoId)

      for(let i = 0; i < this.todos.length; i++) {
        if(this.todos[i].id != i) {
          this.todos[i].id = i;
        }
      }

      this.$emit('pushTodos', this.todos);
    })
  }
};
</script>
<style lang="scss" src="styles/todo.scss" scoped></style>