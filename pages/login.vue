<template>
  <v-row justify="center">
    <v-col lg="6" md="6" sm="6" class="pt-16">
      <v-card>
        <v-card-title>
          <h1 class="display-1">ログイン</h1>
        </v-card-title>
        <v-card-text>
          <v-form>
            <v-text-field 
              label="メールアドレス"
              type="email"
              prepend-icon="mdi-email"
              counter="255"
              v-model="form.email"
              :rules="form.emailRules"
              required
            />
            <v-text-field
              label="パスワード"
              prepend-icon="mdi-lock"
              v-model="form.password"
              v-bind:type="form.showPassword ? 'text' : 'password'"
              v-bind:append-icon="form.showPassword ? 'mdi-eye' : 'mdi-eye-off'"
              @click:append="form.showPassword = !form.showPassword"
              :rules="form.passwordRules"
              required
            />
          <v-card-actions>
            <v-btn @click="login()" color="teal lighten-3">ログイン</v-btn>
          </v-card-actions>
          </v-form>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
  export default {
    data() {
      return {
        form: {
          showPassword: false,
          email: '',
          emailRules: [
            v => !!v || 'メールアドレスを入力してください',
            v => /.+@.+\..+/.test(v) || '有効なメールアドレスではありません',
            v => (!!v && 255 >= v.length) || `255字以内で入力してください`
          ],
          password: '',
          passwordRules: [
            v => !!v || 'パスワードを入力してください'
          ]
        }
      };
    },
    mounted() {
      this.$axios.get('/sanctum/csrf-cookie');
    },
    methods: {
      async login() {
        try {
          const res = await this.$axios.post('http://localhost:9000/api/login', this.form);
          console.log(res);
          console.log('ログインできた');
        } catch(error) {
          console.log(error);
          console.log('失敗');
        }
      }
    }
  };
</script>