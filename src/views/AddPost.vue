<script setup>
import {ref} from "vue";
import { useRouter } from 'vue-router';
import PostForm from "../components/PostForm.vue";

const router = useRouter();
const URL = "https://jayarredondo.xyz:8080/api/posts/create";

let newPost = ref({title: "", body: ""})
const token = localStorage.getItem("access_token");


const addPost = async (post) => {
  console.log("Adding new post: " + post.title + " " + post.rating);
  let resp = await fetch(URL, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Authorization": `Bearer ${token}`
    },
    body: JSON.stringify(post)
  })
  newPost.value = {title: "", body: ""}
  await router.push("/");
}
</script>
<template>
  <div class="container d-flex w-100 justify-content-center">
    <div class="w-50 p-5 neon-border mt-5">
      <form>
        <PostForm v-model="newPost" title="Add post" />
        <button class="btn btn-warning my-3" type="submit" @click.prevent="addPost(newPost)">Add Post</button>
      </form>
    </div>
  </div>
</template>
<style scoped>
.neon-border {
  border: 1px solid white;
  box-shadow: 0 0 15px 5px rgba(255, 255, 0, 0.75);
}
</style>