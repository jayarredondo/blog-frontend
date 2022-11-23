<script setup>
import PostCard from '../components/PostCard.vue';
import Loading from '../components/Loading.vue'
import {ref} from "vue";


const posts = ref([]);
const isLoading = ref(false);
const URL = "http://localhost:3000/posts";

const getAllPosts = async (url) => {
  isLoading.value = true;
  console.log("Fetching posts...");
  posts.value = await fetch(url).then(resp => resp.json());
  console.log(posts.value)
  isLoading.value = false;
  return posts.value;
}

if(posts.value.length <= 0) {
  getAllPosts(URL);
  console.log(posts.value)
}

const deletePost = async (id) => {
  await fetch(URL + "/" + id, {
    method: "DELETE",
    headers: {
      'Content-Type': 'application/json'
    }
  }).then(() => confirm("Are you sure you want to delete?"))
  await getAllPosts(URL);
}

</script>

<template>
  <Loading v-if="isLoading" />
  <main v-if="!isLoading" class="container my-5">
    <div class="row">
      <div class="col-12" v-for="post in posts" :key="post.id">
        <PostCard :post="post" @delete-post="deletePost(post.id)" />
      </div>
    </div>
  </main>
</template>
