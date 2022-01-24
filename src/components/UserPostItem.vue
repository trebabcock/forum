<template>
  <div
    class="flex items-center w-full h-16 bg-slate-900 rounded-lg px-6 py-2 hover:bg-slate-700 shadow-lg"
  >
    <div
      class="flex flex-grow items-center py-2 px-2 truncate cursor-pointer"
      @click="toPost"
    >
      <div class="w-full truncate flex flex-col">
        <span class="text-xl font-bold">{{ post.title }}</span>
        <span class="text-xs text-slate-500">
          Posted {{ time }} in
          <router-link :to="'/board/' + board.id" class="text-slate-400 z-40">{{
            board.name
          }}</router-link>
        </span>
      </div>
    </div>
    <div
      v-if="lastComment.date_time"
      class="border-r flex-shrink-0 border-slate-500 p-2"
    >
      <span class="text-sm">Comments: {{ lastComment.count }}</span>
    </div>
    <div v-else class="border-r flex-shrink-0 border-slate-500 p-2">
      <span class="text-sm">Comments: 0</span>
    </div>
    <div
      v-if="lastComment.date_time"
      class="w-40 p-2 flex-shrink-0 break-normal overflow-hidden text-center text-xs max-h-full"
    >
      {{ commentTime }} by
      <router-link :to="lastCommentUser" class="text-slate-300">{{
        lastComment.author
      }}</router-link>
    </div>
    <div
      v-else
      class="w-40 p-2 flex-shrink-0 break-normal overflow-hidden text-center text-xs max-h-full"
    >
      <span>No Comments</span>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "UserPostItem",
  props: {
    post: {},
  },
  data() {
    return {
      lastComment: {},
      board: {},
    };
  },
  mounted() {
    this.axios
      .get("/post/" + this.post.id + "/getLastComment")
      .then((response) => {
        if (response.status == 200) {
          this.lastComment = response.data;
        } else {
          console.log(response);
        }
      });
    this.axios.get("/board/fromPost/" + this.post.id).then((response) => {
      if (response.status == 200) {
        this.board = response.data;
      } else {
        console.log(response);
      }
    });
  },
  methods: {
    toPost() {
      this.$router.push("/post/" + this.post.id);
    },
  },
  computed: {
    commentTime() {
      return moment(this.lastComment.date_time).calendar();
    },
    time() {
      return moment(this.post.create_date).calendar();
    },
    lastCommentUser() {
      return "/user/" + this.lastComment.author;
    },
  },
};
</script>
