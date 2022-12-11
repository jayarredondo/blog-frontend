<script setup>
import PostCard from '../components/PostCard.vue';
import Loading from '../components/Loading.vue'
import {ref} from "vue";
import {useRouter} from "vue-router";

const router = useRouter();
const posts = ref([]);
const isLoading = ref(false);
const URL = "http://jayarredondo.xyz:8080/api/posts";
const token = localStorage.getItem("access_token");
const currentUser = ref();

function getUser() {
  const accessToken = token;
  if (!accessToken) {
    return false;
  }
  const parts = accessToken.split('.');
  const payload = parts[1];
  const decodedPayload = atob(payload);
  const payloadObject = JSON.parse(decodedPayload);
  const user = {
    userName: payloadObject.user_name,
    role: payloadObject.authorities[0]
  }
  return currentUser.value = user;
}

getUser();

const getAllPosts = async (url) => {
  isLoading.value = true;
  console.log("Fetching posts...");
  posts.value = await fetch(url).then(resp => resp.json());
  isLoading.value = false;
  return posts.value;
}

if (posts.value.length <= 0) {
  getAllPosts(URL);
}

const deletePost = async (id) => {
  await fetch(URL + "/" + id + "/delete", {
    method: "DELETE",
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`
    }
  }).then(() => confirm("Are you sure you want to delete?"))
  await getAllPosts(URL);
}

async function removeStaleTokens() {
  console.log("Removing stale tokens...");

  // clear tokens from localStorage if backend tells us the tokens are invalid
  // make the request
  const request = {
    method: 'GET',
    headers: localStorage.getItem("access_token")
        ? {
          'Content-Type': 'application/json', 'Authorization': 'Bearer ' + `${localStorage.getItem("access_token")}`
        } : {'Content-Type': 'application/json'}
  }
  await fetch("http://jayarredondo.xyz:8080/api/users/me", request)
      .then((response) => {
        // if fetch error then you might be using stale tokens
        console.log(response.status)
        if (response.status === 401) {
          window.localStorage.clear();
          router.push("/login")
        }
      }).catch(error => {
        console.log("FETCH ERROR: " + error);
      });
}

removeStaleTokens();

</script>

<template>
  <Loading v-if="isLoading"/>
  <main v-if="!isLoading" class="container my-5">
    <div class="row">
      <div class="col-12" v-for="post in posts" :key="post.id">
        <PostCard :current-user="currentUser" :post="post" @delete-post="deletePost(post.id)"/>
      </div>
    </div>
  </main>
</template>
