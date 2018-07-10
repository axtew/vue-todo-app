<template lang="pug">
  .todo-input
    input(
      type="text"
      placeholder="Задание"
      @keydown.enter="addTodo"
      v-model="todo.name"
    ).input
    input(
      type="text"
      placeholder="Описание"
      @keydown.enter="addTodo"
      v-model="todo.desc"
    ).input
      
</template>
<script>
export default {
  data() {
    return {
      todo: {
        id: 0,
        name: '',
        desc: '',
        checked: false,
        date: {
          year: 0,
          mounth: 0,
          day: 0,
          hour: 0,
          minute: 0,
          timeZone: 0
        }
      },
    };
  },
  methods: {
    addTodo() {
      if(this.todo.name.length && this.todo.desc.length) {
        let dateNow = new Date();
        this.todo.date.year = dateNow.getFullYear();
        this.todo.date.mounth = dateNow.getMonth() + 1;
        this.todo.date.day = dateNow.getDate();
        this.todo.date.hour = dateNow.getHours();
        this.todo.date.minute = dateNow.getMinutes();
        this.todo.date.timeZone = dateNow.getTimezoneOffset();

        this.$emit('addTodo', {...this.todo});
        this.todo.name = "";
        this.todo.desc = "";
      }
    }
  }
};
</script>
<style lang="scss" scoped src="styles/todoInput.scss"></style>