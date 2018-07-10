<template lang="pug">
  .wrap
    h2.title Вход

    form.form(@submit.prevent="enterUser")
      .form__row
        label.form__label(for="email") Ваш email
        input.form__input#email(
          type="email"
          placeholder="Введите Email"
          required
          v-model="user.email"
        )
      
      .form__row
        label.form__label(for="pass") Ваш пароль
        input.form__input#pass(
          type="password"
          placeholder="Введите пароль"
          required
          v-model="user.pass"
        )

      .form__allert(v-if="error")
        <strong>Упс!</strong> Ошибка.

      .form__btn
        button.btn(type="submit") Войти
      
</template>

<script>
export default {
  data() {
    return {
      user: {
        email: '',
        pass: ''
      },
      error: false
    }
  },
  methods: {
    enterUser() {
      firebase.auth().signInWithEmailAndPassword(this.user.email, this.user.pass)
        .then( response => {
          const settings = {
            email: response.user.email,
            uid: response.user.uid,
            mainPage: true,
            complete: true
          }
          this.$emit('addUser', settings)
        })

    }
  }
}
</script>

<style lang="scss" src="styles/sign.scss" scoped>
  

</style>