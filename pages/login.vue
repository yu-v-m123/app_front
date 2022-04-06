<template>
  <form name='login' @submit.prevent='login()'>
    <label for="email">Email</label>
    <input type="email" id="email" placeholder="メールアドレスを入力してください" v-model='form.email' required />
    <label for="password">Password</label>
    <input type="password" id="password" placeholder="パスワードを入力してください" v-model='form.password' required />
    <button type="submit">送信</button>
  </form>
</template>

<script>
  export default {
    data() {
      return {
        form: {
          email: '',
          password: ''
        },
      };
    },
    mounted() {
      this.$axios.get('/sanctum/csrf-cookie');
    },
    methods: {
      async login() {
        try {
          const res = await this.$auth.loginWith('local', {data: this.form});
          console.log(res);
        } catch(error) {
          console.log(error);
        }
      },
    },
  };
</script>