<template>
  <div
    class="flex items-center w-full h-16 bg-slate-900 rounded-lg px-6 py-2 hover:bg-slate-700 cursor-pointer shadow-lg"
    @click="toPost"
  >
    <div class="flex flex-grow items-center py-2 px-2 truncate">
      <div class="w-full truncate flex flex-col">
        <span class="text-xl font-bold">{{ post.title }}</span>
        <span class="text-xs text-slate-500">
          Posted {{ time }} by
          <router-link
            :to="'/user/' + postAuthor.username"
            class="text-slate-400"
            >{{ postAuthor.username }}</router-link
          >
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
      Last comment {{ commentTime }} by
      <span class="text-slate-300">{{ lastComment.author }}</span>
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
  name: "PostItem",
  props: {
    post: {},
  },
  data() {
    return {
      lastComment: {},
      postAuthor: "",
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
    this.axios.get("/user/fromPost/" + this.post.id).then((response) => {
      if (response.status == 200) {
        this.postAuthor = response.data;
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
  },
};
</script>
