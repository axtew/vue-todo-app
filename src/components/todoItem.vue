<template lang="pug">
  .todo-item
    label.label(:class="{checked: todo.checked}")
      .item-top
        .input-block
          input(
            type="checkbox"
            @change="checkTodo"
            :checked="todo.checked"
          ).input
        .item-text
          .title {{todo.name}}
          .desc {{todo.desc}}
      .item-bottom
        span.create-span.create-year Создан {{todo.date.day}}.{{todo.date.mounth}}.{{todo.date.year}}
        span.create-span.time-after-create ({{formatDate(todo.date.year, todo.date.mounth, todo.date.day, todo.date.hour, todo.date.minute, todo.date.timeZone)}})
    .button
      button(
        type="button"
        @click="removeTodo"
      ).remove
        | x
</template>
<script>
import {eventBus} from './../main';

export default {
  props: {
    todo: Object
  },
  methods: {
    removeTodo() {
      eventBus.$emit('removeTodo', this.todo.id)
    },
    checkTodo(e) {
      const todoItem = {
        ...this.todo,
        checked: e.target.checked
      }
      this.$emit('checkTodo', todoItem)
    },
    formatDate(year, mounth, day, hour, minute, timeZone) {
      let date = new Date(year, mounth - 1, day, hour, minute);

      let dateNow = new Date();

      let timeZoneE = (-dateNow.getTimezoneOffset() - -timeZone) * 60000;

      let diff = dateNow - date - timeZoneE;
      let sec = Math.floor(diff / 1000);
      let min = Math.floor(sec / 60);

      if (min < 1) {
        return 'только что';
      }

      if (min < 60) {
        return min + ' мин. назад';
      }

      let h = Math.floor(min / 60);

      if (h < 24) {
        return h + ' ч. назад';
      }

      let d = Math.floor(h / 24);

      if (d < 30) {
        return h + ' д. назад';
      }

      let m = Math.floor(d / 30);

      if (m < 12) {
        return h + ' м. назад';
      }
    }
  }
}
</script>
<style src="styles/todoItem.scss" lang="scss" scoped></style>