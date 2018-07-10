<template lang="pug">
  #app
    header.header
      .header__desc TODO on Vue.js

      .header__btns(v-if="!signComplete")
        button.btn.btn--enter(type="button" @click="switchSign('sign-in')") Войти
        button.btn(type="button" @click="switchSign('sign-up')") Регистрация

      span.email-value(v-else) {{ email }}

    .container
      .sign(v-if="!isMainPage")
        sign-in(
          v-if="sign === 'sign-in'"
          @addUser="addUser"
        )

        sign-up(
          v-else
          @regSuccess="sign = $event"
        )
      
      todo(v-if="signComplete" :uid="uid" @pushTodos="pushTodos")

</template>

<script>
import signIn from './components/signIn';
import signUp from './components/signUp';
import todo from './components/todo';

export default {
  name: 'app',
  data() {
    return {
      sign: 'sign-in',
      isMainPage: false,
      signComplete: false,
      email: '',
      uid: ''
    }
  },
  components: {
    todo,
    signIn,
    signUp
  },
  methods: {
    switchSign(currentSign) {
      this.sign = currentSign
    },
    addUser(user) {
      this.isMainPage = user.mainPage
      this.signComplete = user.complete
      this.email = user.email
      this.uid = user.uid
    },
    pushTodos(todosList) {
      firebase.database().ref('users/' + this.uid).set({
        todos: todosList
      })
    }
  }
}
</script>

<style lang="scss" scoped>
  .header {
    background-color: #000;
    min-height: 60px;
    padding: 10px 80px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .header__desc {
    color: #fff;
    font-size: 24px;
  }

  .btn {
    border: none;
    border-radius: 5px;
    padding: 5px 10px;
    background-color: #d8d8da;
    cursor: pointer;
    transition: .3s;

    &--enter {
      margin-right: 20px;
    }

    &:hover {
      background-color: darken($color: #d8d8da, $amount: 11%);
    }
  }

  .email-value {
    color: #fff;
    font-size: 18px;
  }

  .sign {
    display: flex;
    justify-content: center;
  }
</style>
