<script setup>

import {ref} from "vue";
import {useRouter} from "vue-router/dist/vue-router";

const router = useRouter();
const username = ref("");
const password = ref("");

const login = async () => {
  console.log("attempting to log in..")
  let obj = {
    username: username.value,
    password: password.value,
    grant_type: 'password'
  }

  await fetch("https://jayarredondo.xyz:8080/oauth/token", {
    method: "POST",
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
      'Authorization': 'Basic ' + btoa('rest-blog-client:secret')
    },
    body: `grant_type=${obj.grant_type}&username=${obj.username}&password=${obj.password}&client_id=rest-blog-client`
  }).then(data => data.json()).then(data => {
    if (data.access_token) {
      console.log("storing token...")
      localStorage.setItem('access_token', data.access_token)
    }
    if (data.refresh_token) {
      localStorage.setItem("refresh_token", data.refresh_token);
      console.log("Refresh token set")
    }
    router.push({name: "home"})
  })
}

</script>
<template>
  <div class="container text-warning">
    <div class="row w-50 mx-auto mt-5 neon-border">
      <h1 class="text-center mt-5 text-warning">Login</h1>
      <form @submit.prevent="login" class="my-3">
      <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Username</label>
        <input v-model="username" type="text" class="form-control" id="exampleInputEmail1"
               aria-describedby="emailHelp">
      </div>
      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Password</label>
        <input v-model="password" type="password" class="form-control" id="exampleInputPassword1">
      </div>
      <button type="submit" class="btn btn-warning form-control">Log In</button>
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