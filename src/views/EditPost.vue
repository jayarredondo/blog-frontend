<script setup>
import {onMounted, ref} from 'vue';
import {useRoute, useRouter} from 'vue-router';
import Loading from "../components/Loading.vue";
import PostForm from "../components/PostForm.vue";

const isLoading = ref(false);
const token = localStorage.getItem("access_token");
const route = useRoute();
const router = useRouter();
const URL = "http://jayarredondo.xyz:8080/api/posts";
const postToEdit = ref({id: null, title: "", rating: null});

const getPostById = async () => {
  isLoading.value = true;
  console.log("Fetching post by ID.")
  postToEdit.value = await fetch(URL + "/" + route.params.id).then(resp => resp.json());
  console.log("Post has been fetched: ", postToEdit.value)
  isLoading.value = false;
  return postToEdit;
}

const editPost = async (post) => {
  await fetch(`${URL}/${post.id}/edit`, {
    method: "PATCH",
    headers: {
      "Content-Type": "application/json",
      "Authorization": `Bearer ${token}`
    },
    body: JSON.stringify(post)
  })
  postToEdit.value = {id: null, title: "", rating: null};
  await router.push({name: "home"});
}

onMounted(() => {
  getPostById();
  console.log(postToEdit.value)
})

</script>
<template>
  <Loading v-if="isLoading" />
  <main v-if="!isLoading">
    <div class="container d-flex w-100 justify-content-center">
      <div class="w-50 p-5 neon-border mt-5">
        <PostForm v-model="postToEdit" title="Edit Post" />
        <button class="btn btn-warning mt-3" @click.prevent="editPost(postToEdit)">Submit Changes</button>
      </div>
    </div>
  </main>
</template>
<style scoped>
.neon-border {
  border: 1px solid white;
  box-shadow: 0 0 15px 5px rgba(255, 255, 0, 0.75);
}
</style>