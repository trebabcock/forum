<template>
  <nav class="flex items-center justify-between flex-wrap bg-slate-700 p-6 rounded-lg space-x-8">
    <div class="flex items-center flex-shrink-0 text-slate-50 px-4">
      <router-link class="text-xl font-bold tracking-widest uppercase" to="/topics">Kerr Forum</router-link>
    </div>
    <div class="flex items-center justify-center flex-grow">
      <div
        class="flex p-2 space-x-2 rounded-lg w-full shadow-lg"
        v-bind:class="{
          'bg-slate-500': asdf,
          'bg-slate-600': !asdf,
        }"
      >
        <input
          class="w-full rounded px-4 py-2 bg-slate-600 focus:bg-slate-500 outline-none text-red-50 flex-grow"
          @hover="asdf = true"
          @focus="asdf = true"
          @blur="asdf = false"
          type="text"
          placeholder="Search..."
          id="searchForum"
        />
        <div
          class="rounded-lg px-4 py-2 flex items-center text-center bg-slate-700 hover:bg-slate-500 text-slate-50 cursor-pointer shadow-lg"
          type="submit"
          id="submit"
          name="submit"
        >
          <span class="text-slate-50 font-bold">Search</span>
        </div>
      </div>
    </div>

    <!--<user-dropdown v-if="loggedIn" />-->
    <profile-dropdown v-if="loggedIn" />
    <div
      v-else
      @click="goToLogin"
      class="rounded-lg px-4 py-2 flex items-center text-center bg-slate-600 hover:bg-slate-500 text-slate-50 cursor-pointer"
    >
      <span class="text-lg font-bold">Sign In</span>
    </div>
  </nav>
</template>

<script>
//import UserDropdown from "./UserDropdown.vue";
import ProfileDropdown from "./ProfileDropdown.vue";
export default {
  name: "TopBar",
  data() {
    return {
      asdf: false,
    };
  },
  components: {
    //UserDropdown,
    ProfileDropdown,
  },
  methods: {
    goToLogin() {
      this.$router.push("/login");
    },
  },
  computed: {
    loggedIn: function () {
      return this.$store.getters.getLoggedIn;
    },
    user: function () {
      return this.$store.getters.getUser;
    },
    searchFocused: function () {
      var searchBar = document.getElementById("searchForum");
      return document.activeElement === searchBar;
    },
  },
};
</script>
