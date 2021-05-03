<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <ReposList msg="EpicMax Jedi you are welcome!"/>
  </div>
</template>

<script>
// @ is an alias to /src
import ReposList from '@/components/ReposList.vue'

export default {
  name: 'Home',
  components: {
    ReposList
  }
}
</script>
