<script setup>
import {ref} from "vue";
import {useRouter} from "vue-router";

// THIS NEEDS WORK

const isLoggedIn = ref();
const router = useRouter()

if(localStorage.getItem("access_token")) {
  isLoggedIn.value = true;
} else {
  isLoggedIn.value = false;
}

const logoutHandler = () => {
  localStorage.clear();
  isLoggedIn.value = false;
  router.push("/login");
}

</script>
<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-warning">
    <div class="container-fluid">

      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent"
              aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <RouterLink class="nav-link active" aria-current="page" to="/">Posts</RouterLink>
          </li>
          <li class="nav-item">
            <RouterLink class="nav-link" to="/addPost">Add Post</RouterLink>
          </li>
          <li class="nav-item">
            <RouterLink class="nav-link" to="/about">About</RouterLink>
          </li>
        </ul>
        <form class="d-flex">
          <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
          <button class="btn btn-outline-success" type="submit">Search</button>
        </form>
        <ul class="navbar-nav">
          <li class="nav-item" v-if="!isLoggedIn">
            <RouterLink class="nav-link" to="/login">Login</RouterLink>
          </li>
          <li class="nav-item" v-if="isLoggedIn">
            <button class="nav-item" @click="logoutHandler">Logout</button>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>