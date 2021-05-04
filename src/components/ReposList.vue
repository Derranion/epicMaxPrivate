<template>
  <div class="reposList">
    <h2>Here is the list of the most popular Git repositories:</h2>
   
    <div v-if='!loading' class='repository'>
      <button :disabled='currentPage<2' 
              @click='renderPrev()' 
              class='controlBtn'>Prev</button>
      <button @click='renderNext()'
              class='controlBtn'>Next</button>
      <ul>
        <RepoItem v-for="(repo,i) of repos"
                  v-bind:key="i"
                  v-bind:repo="repo"
        />
      </ul>
    </div>
    <div v-if="loading" class="lds-dual-ring"></div>

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
      currentPage: 1,
      loading: false
    }
  },
  created(){
    this.fetchPage(this.currentPage)
  },
  methods: {
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
    capitalizeFirst(inp){
            return inp[0].toUpperCase()+inp.slice(1)
    },
    fetchPage(page){
      this.loading=true;
      fetch(this.generateReq(page))
      .then(res=>res.json())
      .then(res=>{
        this.repos=[];
        (res.items||[]).forEach(rep=>{
          this.repos.push({
              name: this.capitalizeFirst(rep.name),
              link: rep.html_url,
              owner: this.capitalizeFirst(rep.owner.login),
              forks: rep.html_url+'/network/members',
              issues: rep.html_url+'/issues',
              watchers: rep.watchers_count
            })
        })
      }).then(()=> this.loading = false)
    }
  }
}
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 10px;
  max-width: 100%;
  height: 50vh;
  overflow: hidden;
  overflow-y: initial;
}
.controlBtn {
  font-size: 150%;
  background-color: rgb(254 155 255);
}
h2 {
  margin: 20px;
  color: white;
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
  color: rgb(255, 255, 255);
  content: "Epic";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 8px solid rgb(255, 255, 255);
  border-color: rgb(254 155 255) transparent  rgb(254 155 255) transparent;
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
</style>
