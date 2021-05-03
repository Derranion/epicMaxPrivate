<template>
  <div class="reposList">
    <h1>{{ msg }}</h1>
    <div class="lds-dual-ring"></div>
    <h2>Below are all git repositories</h2>
    <!-- <h3>
        <span>Name</span>
        <span>Link</span>
        <span>Owner</span>
        <span>Forks</span>
        <span>Issues</span>
        <span>Watchers</span>
    </h3> -->
    <ul>
      <!-- name: "grit"

owner.html_url: 'https://git...owner'

owner.login 'momojombo'

forks_url: "https://api.github.com/repos/mojombo/grit/forks"

issues_url: "https://api.github.com/repos/mojombo/grit/issues

watchers https://api.github.com/repos/octocat/hello-world/subscribers

https://api.github.com/repos/mojombo/grit/subscribers -->
      <RepoItem v-for='(repo,i) of repos'
                v-bind:key="i"
                v-bind:repo='repo'
      />
    </ul>
  </div>
</template>

<script>
import RepoItem from './RepoItem'

export default {
  name: 'ReposList',
  props: {
    msg: String
  },
  components: {
    RepoItem
  },
  data() {
    return {
      repos: []
    }
  },
  created(){
    fetch('https://api.github.com/repositories')
      .then(res=>res.json())
      .then(res=>{
        // iterate through all objects from the response
        res.forEach(rep=>{
          console.log(res)
          this.repos.push({
            name: rep.name,
            link: rep.owner.html_url,
            owner: rep.owner.login,
            forks: rep.forks_url,
            issues: rep.issues_url,
            watchers:5
          })
        })
      })
    // get repos from github using fetch
  },
  methods: {
    scrolled(){
      console.log('scrolled')
    }
  }
}
</script>

<style scoped>
ul {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  list-style-type: none;
  padding: 0;
  max-width: 100%;
}
h1 {
  margin: 10px;
}
h2 {
  margin: 20px;
}
h3 {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
}
.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: "Epic";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 8px solid rgb(255, 255, 255);
  border-color: #fff transparent #fff transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
/* .lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: "Epic";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 8px solid rgb(255, 255, 255);
  border-color: #fff transparent #fff transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
} */
</style>
