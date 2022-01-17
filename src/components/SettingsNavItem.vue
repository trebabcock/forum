<template>
  <router-link
    class="rounded-lg w-full block px-4 py-2 shadow-lg"
    :to="route"
    v-bind:class="{
      'bg-slate-500': isThisPage,
      'bg-slate-700 hover:bg-slate-600': !isThisPage,
    }"
  >
    <span class="text-slate-50 font-bold text-sm">
      <slot></slot>
    </span>
  </router-link>
</template>

<script>
export default {
  props: {
    route: {
      type: String,
      required: true,
    },
  },
  mounted() {},
  computed: {
    isThisPage() {
      return this.route === this.$route.path;
    },
  },
};
</script>
