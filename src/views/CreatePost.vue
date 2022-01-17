<template>
  <div class="flex items-center text-slate-50 justify-center h-full w-full">
    <div class="flex flex-col space-y-4 w-96 items-center">
      <input
        class="px-4 w-full py-2 bg-slate-800 rounded-lg outline-none hover:bg-slate-600 focus:bg-slate-600 shadow-lg"
        type="text"
        placeholder="Title"
        v-model="postTitle"
        id="post-title"
      />
      <textarea
        class="p-4 w-full bg-slate-800 hover:bg-slate-600 rounded-lg outline-none focus:bg-slate-600 shadow-lg h-64"
        placeholder="Body"
        v-model="postBody"
        id="post-body"
      ></textarea>
      <div
        class="rounded-lg h-10 flex py-2 px-4 max-w-min items-center justify-center bg-slate-800 hover:bg-slate-600 cursor-pointer shadow-lg"
        @click="createPost"
      >
        <span>Create</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CreatePost",
  data() {
    return {
      postTitle: "",
      postBody: "",
    };
  },
  methods: {
    createPost() {
      var post = {
        title: this.postTitle,
        content: this.postBody,
      };
      this.$store.dispatch("createPost", [this.$route.params.id, post]);
    },
  },
};
</script>
