<template>
  <div class="md-layout md-alignment-center-center" style="height: 100vh;">
    <md-card class="md-layout-item md-size-50">
      <md-card-header>
        <div class="md-title">ユーザー登録</div>
      </md-card-header>

      <form @submit.prevent="resisterUser">
        <md-card-content>
          <md-field md-clearable>
            <label for="emal">メールアドレス</label>
            <md-input type="email" name="email" id="email" autocomplete="email"
            v-model="form.email" />
          </md-field>
          <md-field>
            <label for="password">パスワード</label>
            <md-input type="password" name="password" id="password"
            autocomplete="password" v-model="form.password" />
          </md-field>
        </md-card-content>

        <md-card-actions>
          <md-button to="/login">ログイン</md-button>
          <md-button class="md-primary md-raised" type="submit">登録</md-button>
        </md-card-actions>
      </form>
    </md-card>
  </div>
</template>

<script>
  export default {
    data: () => ({
      form: {
        email: '',
        password: ''
      }
    }),
    methods: {
      async resisterUser() {
        await this.$store.dispatch('authenticateUser', {
          email: this.form.email,
          password: this.form.password,
          returnSecureToken: true
        })
      }
    }
  }
</script>