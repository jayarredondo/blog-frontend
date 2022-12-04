<script setup>
import {RouterView} from 'vue-router'
import TopNav from "./components/TopNav.vue";

const login = async () => {
  console.log("attempting to log in..")
  let obj = {
    username: 'jayman123',
    password: 'groovy',
    grant_type: 'password'
  }

  await fetch("http://localhost:8080/oauth/token", {
    method: "POST",
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
      'Authorization': 'Basic ' + btoa('rest-blog-client:secret')
    },
    body: `grant_type=${obj.grant_type}&username=${obj.username}&password=${obj.password}&client_id=rest-blog-client`
  }).then(data => {
    console.log(data);
    if(data.route['access_token']) {
      localStorage.setItem('access_token', data.route['access_token'])
    }
    if (data.route['refresh_token']) {
      localStorage.setItem("refresh_token", data.route['refresh_token']);
      console.log("Refresh token set")
    }
  });
}

login();

</script>

<template>
  <TopNav />
  <RouterView/>
</template>
