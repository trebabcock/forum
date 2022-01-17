<template>
  <div class="flex justify-center w-full">
    <div class="flex flex-col space-y-4 w-2/6 justify-center">
      <div class="bg-slate-800 flex-grow rounded-lg shadow-lg w-full p-4 flex flex-col space-y-4">
        <span class="font-bold text-lg">{{ post.title }}</span>
        <span class="whitespace-pre-line">{{ post.content }}</span>
        <span class="text-xs text-slate-500">
          Posted {{ time }} by
          <router-link :to="'/user/' + author.username" class="text-slate-400">{{ author.username }}</router-link>
        </span>
      </div>
      <div class="flex flex-col space-y-2">
        <textarea
          class="p-4 w-full bg-slate-800 hover:bg-slate-600 rounded-lg outline-none focus:bg-slate-600 shadow-lg h-24"
          placeholder="Add Comment"
          v-model="comment"
          id="comment"
        ></textarea>
        <div
          class="rounded-lg h-10 flex py-2 px-4 max-w-min items-center justify-center bg-slate-800 hover:bg-slate-600 cursor-pointer shadow-lg"
          @click="addComment"
        >
          <span>Create</span>
        </div>
        <span class="text-2xl font-bold text-slate-500">Comments</span>
        <div class="flex flex-col space-y-2">
          <Comment v-for="comment in comments" :key="comment.id" :comment="comment" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Comment from "../components/Comment.vue";
import moment from "moment";

export default {
  name: "Post",
  data() {
    return {
      comment: "",
      id: this.$route.params.id,
    };
  },
  components: {
    Comment,
  },
  mounted() {
    this.$nextTick(function () {
      this.$store.dispatch("getPost", this.id);
      this.$store.dispatch("getCommentsFromPost", this.id);
      this.$store.dispatch("getPostAuthor", this.id);
    });
  },
  methods: {
    addComment: function () {
      if (!this.loggedIn) {
        this.$router.push("/login");
        return;
      }
      if (!this.comment) {
        return;
      }
      var newComment = {
        post_id: this.post.id,
        content: this.comment,
      };
      this.comment = "";
      this.$store.dispatch("addComment", newComment);
    },
  },
  computed: {
    post() {
      return this.$store.getters.getPost;
    },
    comments() {
      return this.$store.getters.getComments;
    },
    author() {
      return this.$store.getters.getViewUser;
    },
    time() {
      return moment(this.post.create_date).calendar();
    },
    loggedIn: function () {
      return this.$store.getters.getLoggedIn;
    },
  },
};
</script>
