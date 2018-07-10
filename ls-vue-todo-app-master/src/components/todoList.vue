<template lang="pug">
  .todo-list(v-if="todos.length")
    .content
      ul.list
        li.item(v-for="todo in filteredItems")
          todo-item(
            :todo="todo"
            @checkTodo="checkTodo"
          )
    .footer
      .footer-content
        .counter
          | Кол-во заданий: {{filteredItems.length}}
        .filter
          button(
            v-for="filter in filters"
            type="button"
            @click="filterItems(filter)"
            :class="{active: currentFilter == filter}"
          ) {{filter}}
</template>
<script>
import todoItem from "./todoItem";

export default {
  components: {
    todoItem
  },
  props: {
    todos: Array
  },
  data() {
    return {
      filters: ['All', 'Active', 'Compleated'],
      currentFilter: 'All'
    }
  },
  computed: {
    filteredItems() {
      switch(this.currentFilter) {
        case 'All' :
          return this.todos

        case 'Active' :
          return this.todos.filter(item => item.checked === false)

        case 'Compleated' :
          return this.todos.filter(item => item.checked)
      }
    }
  },
  methods: {
    checkTodo(todo) {
      this.$emit('checkTodo', todo)
    },
    filterItems(filterName) {
      this.currentFilter = filterName
    }
  }
}
</script>
<style src="styles/todoList.scss" lang="scss" scoped></style>