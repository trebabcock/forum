<template>
  <div class="h-screen w-screen flex flex-col bg-slate-900 overflow-x-hidden">
    <div class="bg-transparent p-2 px-6 py-2">
      <top-bar />
    </div>
    <div class="flex flex-grow bg-transparent py-2 px-6">
      <div
        class="flex flex-col w-full h-full bg-slate-700 text-slate-50 shadow-md rounded-lg p-4 space-y-2"
      >
        <div v-if="loading" class="rounded-lg bg-slate-900 w-64 h-64"></div>
        <router-view v-else />
      </div>
    </div>
  </div>
</template>

<script>
import TopBar from "../components/TopBar.vue";
export default {
  components: {
    TopBar,
  },
  computed: {
    loading() {
      return this.$store.getters.getLoading;
    }
  }
};
</script>
