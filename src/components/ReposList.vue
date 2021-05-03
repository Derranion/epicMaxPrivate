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
    <div class='repository'>
      <button @click='renderPrev()'>Prev</button>
      <button @click='renderNext()'>Next</button>
      <ul>
        <RepoItem v-for='(repo,i) of repos'
                  v-bind:key="i"
                  v-bind:repo='repo'
        />
      </ul>
    </div>
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
      repos: [],
      currentPage: 1
    }
  },
  created(){
    //https://api.github.com/repositories
    this.fetchPage(this.currentPage)
    // get repos from github using fetch
  },
  methods: {
    scrolled(){
      console.log('scrolled')
    },
    generateReq(page, perPage=10){
      return `https://api.github.com/search/repositories?q=language:js&per_page=${perPage}&page=${page}&sort=stars&sort=desc`
    },
    renderNext(){
      ++this.currentPage;
      this.fetchPage(this.currentPage)
    },
    renderPrev(){
      if(this.currentPage>1){
        --this.currentPage;
        this.fetchPage(this.currentPage)
      }
    },
    fetchPage(page){
      fetch(this.generateReq(page))
      .then(res=>res.json())
      .then(res=>{
        // iterate through all objects from the response
        this.repos=[];
        res.items.forEach(rep=>{
          console.log(res)
          this.repos.push({
              name: rep.name,
              link: rep.html_url,
              owner: rep.owner.login,
              forks: rep.forks_url,
              issues: rep.issues_url,
              watchers: rep.watchers_count
            })
        })
      })
    }
  }
}
</script>

<style scoped>
ul {
  border: 2px solid purple;
  /* display: flex; */
  justify-content: center;
  align-items: center;
  flex-direction: column;
  list-style-type: none;
  padding: 10px;
  max-width: 100%;
  height: 300px;
  overflow: hidden;
  overflow-y: scroll;
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
