<template>
  <div class="h-screen w-screen flex flex-col bg-slate-900 text-slate-50">
    <div class="bg-transparent w-full h-full p-2">
      <div class="w-full flex justify-center items-center h-full bg-slate-700 rounded-lg">
        <div class="w-64 h-80 flex flex-col space-y-4 items-center justify-center p-4">
          <span class="text-2xl font-bold">Sign In</span>
          <form
            class="flex flex-col justify-center items-center space-y-4 w-full"
            id="login"
            @submit.prevent="handleLogin"
          >
            <input
              class="rounded-lg outline-none w-full h-10 px-2 py-1 bg-slate-800 hover:bg-slate-600 focus:bg-slate-600 shadow-lg"
              type="text"
              placeholder="Email"
              id="email"
              v-model="email"
            />
            <input
              class="rounded-lg outline-none w-full h-10 px-2 py-1 bg-slate-800 hover:bg-slate-600 focus:bg-slate-600 shadow-lg"
              type="password"
              placeholder="Password"
              id="password"
              v-model="password"
            />
            <input
              class="rounded-lg h-10 px-4 py-1 flex items-center justify-center shadow-lg bg-slate-800 hover:bg-slate-600 cursor-pointer"
              type="submit"
              id="submit"
              name="submit"
              value="Sign In"
            />
          </form>
          <span class="text-xs text-slate-400">
            No account? Sign up
            <router-link class="text-indigo-400" to="/register">here.</router-link>
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: null,
      password: null,
    };
  },
  methods: {
    handleLogin: function (e) {
      var user = {
        email: this.email,
        password: this.password,
      };

      this.$store.dispatch("handleLogin", user);
      e.preventDefault();
    },
  },
  mounted() {
    if (this.$store.getters.getLoggedIn) {
      this.$router.push("/");
    }
  },
};
</script>
