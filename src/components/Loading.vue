<template>
  <div class="loader bg-dark">
  </div>
</template>

<script setup>

</script>

<style scoped>
.loader{
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: 9999;
  background: url('../assets/img/Rocket-Loading-Screen.gif')
  50% 50% no-repeat rgb(249,249,249);
}
</style>