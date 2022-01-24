<template>
  <div class="flex items-center justify-center w-full h-full">
    <div class="w-full h-full pl-4 flex flex-col items-center space-y-4">
      <div
        class="p-2 flex flex-col justify-between w-1/2 rounded-lg shadow-lg bg-slate-800"
      >
        <span class="text-2xl mb-2">Change Email</span>
        <input
          type="email"
          class="px-4 py-2 w-full bg-slate-700 mb-2 hover:bg-slate-600 rounded-lg outline-none focus:bg-slate-600 shadow-lg"
          placeholder="New Email"
          v-model="new_email"
          id="new_email"
        />
        <input
          type="password"
          class="px-4 py-2 w-full bg-slate-700 mb-2 hover:bg-slate-600 rounded-lg outline-none focus:bg-slate-600 shadow-lg"
          placeholder="Password"
          v-model="password"
          id="password"
        />
        <div class="w-full flex justify-end space-x-4">
          <button
            class="flex rounded-lg shadow-lg px-4 py-2 bg-slate-700 hover:bg-slate-600 text-slate-50"
            @click="saveEmail"
          >
            Save
          </button>
          <button
            class="flex rounded-lg shadow-lg px-4 py-2 bg-slate-700 hover:bg-slate-600 text-slate-50"
            @click="clearFields"
          >
            Discard
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AccountSettings",
  data() {
    return {
      new_email: "",
      password: "",
    };
  },
  methods: {
    clearFields() {
      document.getElementById("new_email").value = "";
      document.getElementById("password").value = "";
    },
    saveEmail() {
      let user = { email: this.new_email };
      this.axios
        .put("/user/" + this.user.public_user.id, user)
        .then((response) => {
          if (response.status == 200) {
            this.$store.commit("setUser", response.data);
          } else {
            console.log(response);
          }
        });
    },
  },
  computed: {
    user() {
      return this.$store.getters.getUser;
    },
  },
};
</script>
