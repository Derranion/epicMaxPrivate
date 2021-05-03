<template>
    <li class='repo'>
        <div class='name'>{{repo.name}}</div>
        <div class='link'><a :href="repo.link">Link</a></div>
        <div class='owner'>{{repo.owner}}</div>
        <div class='forks'><a :href="repo.forks">Forks</a></div>
        <div class='issues'><a :href="repo.issues">Issues</a></div>
        <div class='watchers'>{{repo.watchers}}</div>
    </li>
</template>

<script>
// split link by / and return last one "mojombo"
// issues split by api.github.com/repos/mojombo/grit/issues{/number}

export default {
    props: {
        repo: Object
    },
    methods: {
        capitalizeFirst(inp){
            return inp[0].toUpperCase()+inp.slice(1)
        }
    },
    beforeMount: function(){
        this.repo.name = this.capitalizeFirst(this.repo.name);
        this.repo.owner = this.capitalizeFirst(this.repo.owner);
        this.repo.issues = this.repo.issues.split('{')[0];
    }
}
</script>

<style>
li.repo {
    margin: 10px auto;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    /* background-color: pink; */
    font-size: 150%;
    /* border-top: 3px solid pink; */
    /* border-bottom: 3px solid pink; */
    box-shadow: 5px 5px 5px rgb(0 0 0 / 70%);
    width: fit-content;
}
div {
    padding: 0px 10px;
    display: block;
}
div.name {
    background-color: rgb(255, 88, 255);
}
div.name:hover {

}
div.link {
    background-color: rgb(255, 148, 250);
}
div.owner{
    background-color: rgb(243, 196, 255);
}
div.forks{
    background-color: rgb(162, 241, 255);
}
div.issues{
    background-color: aqua;
}
div.watchers{
    background-color: rgb(142, 181, 255);
}
</style>