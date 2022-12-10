<script setup>
import {ref} from "vue";

const props = defineProps(['post', 'currentUser']);
const emit = defineEmits(["deletePost"]);

const user = ref();

user.value = props.currentUser;

</script>
<template>
  <div class="card mb-3 bg-dark border-light">
    <div class="card-body">
      <h5 class="card-title">{{ props.post.title }}</h5>
      <p class="card-text">{{ props.post.body }}</p>
      <div v-if="user.userName === props.post.author.username">
      <RouterLink class="btn btn-primary me-3" :to="{name: 'editPost', params: {id: props.post.id}}">Edit
      </RouterLink>
      <button class="btn btn-danger" @click="$emit('deletePost', props.post.id)">Delete</button>
      </div>
    </div>
  </div>
</template>
