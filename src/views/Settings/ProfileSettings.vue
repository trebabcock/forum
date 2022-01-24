<template>
  <div class="flex items-center justify-center w-full h-full">
    <div class="w-full h-full pl-4 flex flex-col items-center space-y-4">
      <div
        class="p-2 flex space-x-4 w-1/2 h-36 rounded-lg shadow-lg bg-slate-800"
      >
        <img
          class="rounded-lg shadow-lg object-cover w-32 h-32"
          :src="avSource"
        />
        <div class="flex flex-col justify-between w-full">
          <span class="text-2xl">Change Avatar</span>
          <input
            type="file"
            class="hidden"
            id="avatar_upload"
            accept=".png,.jpg,.jpeg,.gif"
            @change="updateAvatar"
          />
          <div class="w-full flex justify-between">
            <button
              class="flex items-center rounded-lg shadow-lg px-4 py-2 bg-slate-700 hover:bg-slate-600 text-slate-50"
              @click="openFileSelect"
            >
              <UploadIcon class="w-5 h-5 mr-2" aria-hidden="true" />
              Upload
            </button>
            <!--<button
              class="flex rounded-lg shadow-lg px-4 py-2 mr-4 bg-slate-700 hover:bg-slate-600 text-slate-50"
            >
              <LinkIcon class="w-5 h-5 mr-2" aria-hidden="true" />
              URL
            </button>-->
            <div class="flex space-x-4">
              <button
                class="flex rounded-lg shadow-lg px-4 py-2 bg-slate-700 hover:bg-slate-600 text-slate-50"
                @click="saveAvatar"
              >
                Save
              </button>
              <button
                class="flex rounded-lg shadow-lg px-4 py-2 bg-slate-700 hover:bg-slate-600 text-slate-50"
                @click="saveAvatar"
              >
                Discard
              </button>
            </div>
          </div>
        </div>
      </div>
      <div
        class="p-2 flex flex-col justify-between w-1/2 h-36 rounded-lg shadow-lg bg-slate-800"
      >
        <span class="text-2xl mb-2">Change Username</span>
        <input
          type="text"
          class="px-4 py-2 w-full bg-slate-700 mb-2 hover:bg-slate-600 rounded-lg outline-none focus:bg-slate-600 shadow-lg"
          :placeholder="this.user.public_user.username"
          v-model="username"
          id="username"
        />
        <div class="w-full flex justify-end space-x-4">
          <button
            class="flex rounded-lg shadow-lg px-4 py-2 bg-slate-700 hover:bg-slate-600 text-slate-50"
            @click="saveUsername"
          >
            Save
          </button>
          <button
            class="flex rounded-lg shadow-lg px-4 py-2 bg-slate-700 hover:bg-slate-600 text-slate-50"
            @click="clearUsername"
          >
            Discard
          </button>
        </div>
      </div>
      <div
        class="p-2 flex flex-col justify-between w-1/2 h-64 rounded-lg shadow-lg bg-slate-800"
      >
        <span class="text-2xl mb-2">Change Bio</span>
        <textarea
          class="p-4 w-full bg-slate-700 mb-2 hover:bg-slate-600 rounded-lg outline-none focus:bg-slate-600 shadow-lg flex flex-grow"
          :placeholder="userBio"
          v-model="bio"
          id="bio"
        ></textarea>
        <div class="w-full flex justify-end space-x-4">
          <button
            class="flex rounded-lg shadow-lg px-4 py-2 bg-slate-700 hover:bg-slate-600 text-slate-50"
            @click="saveBio"
          >
            Save
          </button>
          <button
            class="flex rounded-lg shadow-lg px-4 py-2 bg-slate-700 hover:bg-slate-600 text-slate-50"
            @click="clearBio"
          >
            Discard
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { UploadIcon, LinkIcon } from "@heroicons/vue/solid";

export default {
  name: "ProfileSettings",
  data() {
    return {
      avatarChangeType: "",
      avatarSource: "",
      bio: "",
      username: "",
    };
  },
  components: {
    UploadIcon,
    LinkIcon,
  },
  mounted() {
    this.avatarSource = this.user.public_user.avatar_url;
    this.bio = this.user.public_user.bio;
  },
  methods: {
    clearUsername() {
      document.getElementById("username").value = "";
    },
    clearBio() {
      document.getElementById("bio").value = "";
    },
    saveAvatar() {
      if (!this.avatarChangeType) {
        return;
      }
      if (this.avatarChangeType === "upload") {
        var fc = document.getElementById("avatar_upload");
        if (!fc.files) {
          return;
        }
        var file = fc.files[0];

        let formData = new FormData();
        formData.append("file", file);

        this.axios
          .post("/user/" + this.user.public_user.id + "/avatar", formData, {
            headers: {
              "Content-Type": "multipart/form-data",
            },
          })
          .then((response) => {
            if (response.status == 200) {
              this.$store.commit("setUser", response.data);
            } else {
              console.log(response);
            }
          });
      }
    },
    updateAvatar() {
      var fc = document.getElementById("avatar_upload");
      if (!fc.files) {
        return;
      }
      this.avatarSource = URL.createObjectURL(fc.files[0]);
      this.avatarChangeType = "upload";
    },
    openFileSelect() {
      var fc = document.getElementById("avatar_upload");
      fc.click();
    },
    saveUsername() {
      let user = { username: this.username };
      this.axios
        .put("/user/" + this.user.public_user.id, user)
        .then((response) => {
          if (response.status == 200) {
            this.$store.commit("setUser", response.data);
          } else {
            console.log(response);
          }
        });
    },
    saveBio() {
      let user = { bio: this.bio };
      this.axios
        .put("/user/" + this.user.public_user.id, user)
        .then((response) => {
          if (response.status == 200) {
            this.$store.commit("setUser", response.data);
          } else {
            console.log(response);
          }
        });
    },
  },
  computed: {
    user() {
      return this.$store.getters.getUser;
    },
    avSource() {
      return this.avatarSource;
    },
    userbio() {
      return this.bio;
    },
  },
};
</script>
