<template>
  <v-card class="elevation-12">
    <v-toolbar dark color="primary">
      <v-toolbar-title>ログイン</v-toolbar-title>
    </v-toolbar>
    <v-card-text>
      <v-form>
        <v-select
          label="お名前"
          item-text="name"
          item-value="id"
          :items="accounts('ALL')"
          :value="id"
          @input="$store.dispatch('login/id', $event)"
        />
        <v-text-field label="ユーザーID" v-model="user"></v-text-field>
        <v-text-field type="password" v-model="password" label="パスワード"></v-text-field>
      </v-form>
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-btn nuxt to="/signin" color="success">口座開設</v-btn>
      <v-btn nuxt @click="login" color="primary">ログイン</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import { mapGetters } from "vuex";
import md5 from "blueimp-md5";

export default {
  layout: "login",
  data: () => ({
    user: "",
    password: "",
  }),
  computed: {
    ...mapGetters("accounts", ["accounts"]),
    ...mapGetters("login", ["id"]),
  },
  methods: {
    login() {
      const account = this.accounts("ALL").find(
        account => account.user === this.user,
      );
      if (account === undefined) {
        alert("ユーザーIDが違います");
      } else if (account.password === md5(this.password)) {
        this.$router.push("/");
      } else {
        alert("パスワードが違います");
      }
    },
  },
};
</script>
