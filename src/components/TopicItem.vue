<template>
  <div
    class="flex items-center w-full h-16 bg-slate-800 shadow-lg rounded-lg px-6 py-2 hover:bg-slate-600"
  >
    <div
      class="flex flex-grow items-center py-2 px-2 truncate h-full cursor-pointer"
      @click="toPost"
    >
      <div class="w-full truncate flex flex-col">
        <span class="text-xl font-bold">{{ board.name }}</span>
        <span class="text-xs text-slate-500 truncate">
          {{ board.description }}
        </span>
      </div>
    </div>
    <div
      v-if="lastPost.date_time"
      class="border-r border-slate-500 p-2 flex-shrink-0"
    >
      <span class="text-sm">Posts: {{ lastPost.count }}</span>
    </div>
    <div v-else class="border-r border-slate-500 p-2 flex-shrink-0">
      <span class="text-sm">Posts: 0</span>
    </div>
    <div
      v-if="lastPost.date_time"
      class="w-40 p-2 break-normal overflow-hidden text-xs text-center max-h-full flex-shrink-0"
    >
      {{ time }} by
      <router-link :to="getRoute" class="text-slate-300">
        {{ lastPost.author }}
      </router-link>
    </div>
    <div
      v-else
      class="w-40 p-2 break-normal overflow-hidden text-xs max-h-full flex items-center justify-center flex-shrink-0"
    >
      <span>No Posts</span>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "TopicItem",
  props: {
    board: {},
  },
  data() {
    return {
      last_post: {},
    };
  },
  mounted() {
    this.axios
      .get("/board/" + this.currentBoard.id + "/lastPost")
      .then((response) => {
        if (response.status == 200) {
          this.last_post = response.data;
        } else {
          console.log(response);
        }
      });
  },
  methods: {
    toPost() {
      this.$router.push("/topics/" + this.currentBoard.id);
    },
  },
  computed: {
    currentBoard() {
      return this.board;
    },
    lastPost() {
      return this.last_post;
    },
    time() {
      return moment(this.lastPost.date_time).calendar();
    },
    getRoute() {
      return "/user/" + this.lastPost.author;
    },
  },
};
</script>
