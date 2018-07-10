<template lang="pug">
  .wrap
    h2.title Регистрация
    
    form.form(@submit.prevent="registerUser")
      .form__row
        label.form__label(for="email") Ваш email
        input.form__input#email(
          type="email"
          placeholder="Введите Email"
          required
          v-model="user.email"
        )
      
      .form__row
        label.form__label(for="pass") Придумайте пароль (минимум 6 символов)
        input.form__input#pass(
          type="password"
          placeholder="Введите пароль"
          required
          v-model="user.pass"
        )
      
      .form__row
        label.form__label(for="confirm-pass") Подтвердите пароль
        input.form__input#confirm-pass(
          type="password"
          placeholder="Подтвердите пароль"
          required
          v-model="user.confirmPass"
        )

      .form__allert(v-if="error")
        <strong>Упс!</strong> Ошибка.

      .form__btn
        button.btn(type="submit") Регистрация

</template>

<script>
export default {
  name: "sign-up",
  data() {
    return {
      user: {
        email: '',
        pass: '',
        confirmPass: ''
      },
      error: false
    }
  },
  methods: {
    registerUser() {
      if(this.user.pass !== this.user.confirmPass || this.user.pass.length < 6) {
        this.error = true;
      } else {
        firebase.auth().createUserWithEmailAndPassword(this.user.email, this.user.pass)
          .then( () => {
            this.$emit('regSuccess', 'sign-in');
          })
          .catch(error => {
            console.log(error);
          })
      }
    }
  }
}
</script>


<style lang="scss" src="styles/sign.scss" scoped></style>

