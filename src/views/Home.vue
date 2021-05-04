<template>
  <div class="home">
    <img alt="EpicMax logo" src="../assets/logo.png">
    <h1>EpicMax Jedi you are welcome!</h1>
    <ReposList msg=""/>
  </div>
</template>

<script>
import ReposList from '@/components/ReposList.vue'

export default {
  name: 'Home',
  components: {
    ReposList
  }
}
</script>

<style>
h1 {
  margin: 10px;
  color: white;
}
</style>