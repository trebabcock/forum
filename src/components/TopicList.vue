<template>
  <div class="flex flex-col space-y-4">
    <div v-if="!boards" class="flex justify-center items-center">
      <span>Loading...</span>
    </div>
    <topic-item
      v-else
      v-for="board in boards"
      :key="board.id"
      :board="{
        name: board.name,
        id: board.id,
        description: board.description,
      }"
    ></topic-item>
  </div>
</template>

<script>
import TopicItem from "./TopicItem.vue";
export default {
  name: "TopicList",
  components: {
    TopicItem,
  },
  mounted() {
    this.$store.dispatch("fetchBoards");
  },
  computed: {
    boards() {
      return this.$store.getters.getBoards;
    },
  },
};
</script>
