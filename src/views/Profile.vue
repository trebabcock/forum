<template>
  <div class="flex flex-col w-full h-full space-y-4">
    <div
      class="rounded-lg w-full h-36 bg-slate-800 shadow-lg flex items-center p-4 space-x-6"
    >
      <img
        class="rounded-lg h-full object-cover shadow-lg"
        :src="viewUser.avatar_url"
      />
      <div class="flex flex-col h-full flex-shrink-0 space-y-4">
        <span class="text-xl">{{ viewUser.username }}</span>
        <div class="flex flex-col text-xs text-slate-400 space-y-1">
          <span>Posts: {{ userPosts.length }}</span>
          <span>Comments: {{ userComments.length }}</span>
          <span>Points: 196</span>
        </div>
      </div>
      <div class="h-full border-r-2 border-slate-900"></div>
      <div class="overflow-y-auto h-full px-2">
        <span class="overflow-y-auto min-h-full overflow-ellipsis">
          {{ viewUser.bio }}
        </span>
      </div>
    </div>
    <div
      class="w-full flex flex-col flex-grow py-2 px-4 bg-slate-800 rounded-lg shadow-lg"
    >
      <div class="w-full h-14 py-2 flex space-x-2">
        <div
          class="w-36 h-10 cursor-pointer rounded-lg shadow-lg flex items-center justify-center hover:bg-slate-700"
          v-bind:class="{
            'bg-slate-700': currentTab == 'posts',
            'bg-slate-900': !(currentTab == 'posts'),
          }"
          @click="tabPosts"
        >
          <span class="font-bold">Posts</span>
        </div>
        <div
          class="w-36 h-10 cursor-pointer rounded-lg shadow-lg flex items-center justify-center hover:bg-slate-700"
          v-bind:class="{
            'bg-slate-700': currentTab == 'comments',
            'bg-slate-900': !(currentTab == 'comments'),
          }"
          @click="tabComments"
        >
          <span class="font-bold">Comments</span>
        </div>
      </div>
      <div
        class="w-full h-full flex flex-col flex-grow py-4 space-y-2"
        v-if="currentTab == 'posts'"
      >
        <span v-if="!userPosts">This user has no posts.</span>
        <user-post-item
          v-else
          v-for="post in userPosts"
          :key="post.id"
          :post="{
            title: post.title,
            id: post.id,
            create_date: post.create_date,
          }"
        ></user-post-item>
      </div>
      <div
        class="w-full h-full flex flex-col flex-grow py-4 space-y-2"
        v-else-if="currentTab == 'comments'"
      >
        <span v-if="!userComments">This user has no comments.</span>
        <user-comment-item
          v-else
          v-for="comment in userComments"
          :key="comment.id"
          :comment="{
            content: comment.content,
            id: comment.id,
            create_date: comment.create_date,
            post_id: comment.post_id,
          }"
          :user="viewUser"
        ></user-comment-item>
      </div>
    </div>
  </div>
</template>

<script>
import UserPostItem from "../components/UserPostItem.vue";
import UserCommentItem from "../components/UserCommentItem.vue";
export default {
  data() {
    return {
      tab: "posts",
    };
  },
  components: {
    UserPostItem,
    UserCommentItem,
  },
  mounted() {
    this.$store.dispatch(
      "getPublicUserByUsername",
      this.$route.params.username
    );
    this.$store.dispatch("getPostsFromUser", this.$route.params.username);
    this.$store.dispatch("getCommentsFromUser", this.$route.params.username);
  },
  methods: {
    tabPosts: function () {
      this.tab = "posts";
    },
    tabComments: function () {
      this.tab = "comments";
    },
  },
  computed: {
    currentTab() {
      return this.tab;
    },
    viewUser() {
      return this.$store.getters.getViewUser;
    },
    userPosts() {
      return this.$store.getters.getPosts;
    },
    userComments() {
      return this.$store.getters.getComments;
    },
  },
};
</script>
