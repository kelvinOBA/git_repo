<template>
  <div>
    <div class="navbar__section" >
      <div class="navbar__section__content">
        <div>
          <i class="fa fa-folder-open-o"></i> 
          <p> Overview</p>
        </div>
        <div :class="{ 'tab-active': tabActive === 'repo' }">
          <i class="fa fa-hdd-o"></i>
          <p>Repositories</p>
        </div>
        <div>
         <i class="fa fa-files-o"></i>
          <p>Projects</p>
        </div>
        <div>
          <i class="fa fa-cube"></i>
          <p>Packages</p>
        </div>
      </div>
    </div>

    <div class="container">
      <div class="content__section">
        <div>
          <div class="content__section__user__image "  :style="{ backgroundImage: 'url(' +userData.avatarUrl + ')' }"> </div>

          <h2 class="padd-bott-10 mt-10">{{userData.name}}</h2>
          <p class="padd-bott-10">{{userData.bio}}</p>
          <button class="content__section__edit__btn">Edit profile</button>
        </div>
        <div class="content__section__repo_div">
          <div>
            <input type="text" v-model="filterString" placeholder="Find a repository...." />
          </div>
          <hr />
          <!-- <div v-for="data in data" :key="data.id">
                 <p>{{data.name}}</p>
               </div> -->

          <div class="content__section__project__div" v-for="data in filterRepo(repoData)" :key="data.id" >
            <div class="content__section__project__div__child">
              <a :href="data.url" target="_blank"> 
              <p class="content__section__project__div__repo__name">
               {{data.name}}
              </p>
              </a>
              <p class="content__section__project__div__date">Updated on</p>
            </div>
            <div class="content__section__project__div__child__two">
              <button><i class="fa fa-star-o"></i> Star</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loader: false,
      repoData: [],
      userData:[],
      filterString:'',
      tabActive: 'repo',
    }
  },

  methods: {

    filterRepo(repoData){

 return repoData.filter(data => {
if (data.name.toLowerCase().includes(this.filterString.toLowerCase())){
  return data
}
 })
    },

    getRepos() {
      const query = `
      query {
  user(login: "kelvinOBA") {
    id
    avatarUrl
    name
    bio
 
    repositories(first: 10) {
      nodes {
        name
        createdAt 
        updatedAt 
        url 
      }
    }
  }
} `
let vm = this
      fetch('https://api.github.com/graphql', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          Authorization: 'Bearer ' + process.env.GIT_HUB_KEY,
        },
        body: JSON.stringify({ query }),
      })
        .then((res) => res.json())

        .then(function(res) {
     
        vm.repoData = res.data.user.repositories.nodes
        vm.userData = res.data.user
      });
  //       .then((res) =>{
  //         this.repoData = res.data.user.repositories.nodes,

  // // this.userData = res.data.user
  //         // console.log(res)
  //       }
        
  
 
    },
  },

  mounted() {
    this.getRepos()
  },
}
</script>

<style>

.padd-bott-10{
  padding-bottom: 10px;
}
.padd-top-10{
  padding-top: 10px;
}
.mt-10{
    margin-top: 10px;
}


.navbar__section {
  display: flex;
  justify-content: center;
  background: white;
  min-height: 50px;
  border: 1px solid #f2f2f2;
  position: sticky;
  top: 0px;

}
.navbar__section__content {
  max-width: 800px;
  margin: 0 auto;
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
}
.navbar__section__content div {
  display: flex;
  align-items: center;
  flex-basis: 20%;
  padding-bottom: 40px;
  padding: 0px 20px 10px 20px;
  cursor: pointer;
}
.navbar__section__content p{
  color:#24292e;
}
.navbar__section__content i {
  padding-right: 10px;
}
.tab-active {
  border-bottom: 1.5px solid #f9826c;
  font-weight: 600;
}

.content__section {
  display: flex;
  padding: 20px;
}
.content__section div {
  padding-right: 5%;
}
.content__section__user__image {
  height: 200px;
  width: 200px;
  flex-basis: 30%;
  border-radius: 50%;
  border: 1px solid #f2f2f2;
  background-size: cover;
}
.content__section__edit__btn{
    background: #fafbfc;
  border: 1px solid #e1dfdf;
  border-radius: 3px;
  padding: 5px;
      min-width: 100%;
}
.content__section input {
  min-width: 400px;
  border-radius: 4px;
  min-height: 40px;
  border: 1px solid #d0d0d0;
  margin-bottom: 20px;
  min-height: 35px;
  padding: 5px;
}
.content__section__repo_div {
  flex-basis: 80%;
}
.content__section__project__div {
  min-height: 100px;
  border-bottom: 1px solid #f2f2f2;
  display: flex;
  align-items: center;
}
.content__section__project__div div {
  flex-basis: 50%;
}
.content__section__project__div__child__two {

  display: flex;
  justify-content: flex-end;
}
.content__section__project__div__child__two button {
  background: #fafbfc;
  border: 1px solid #e1dfdf;
  border-radius: 3px;
  padding: 5px;
}
.content__section__project__div__repo__name {
  color: #0366d6;
  font-weight: 600;
  font-size: 19px;
  padding-bottom: 12px;
  cursor: pointer;
}
a{
  text-decoration: none;
}
.content__section__project__div__date {
  color: #586069;
  font-size: 12px;
}
hr {
  border-top: 1px solid #f2f2f2;
}
.container {
  min-height: 100vh;
  padding-left: 10%;
  padding-right: 10%;
}

@media(max-width: 769px){
.container {
  min-height: 100vh;
  padding-left: 5%;
  padding-right: 5%;
      min-width: fit-content;
}

  .content__section{
    flex-direction: column;
    min-width: fit-content;
    padding: 0px;
    margin-top:20px;
  }
  .content__section__user__image {
flex-basis: 100%;
margin: 0 auto;

  }
  .content__section input {
margin-top: 20px;
    min-width: 100%;
  }
  .content__section div {

    padding-right:0%;
  }
}


@media(max-width: 520px){

.navbar__section {

  min-width:100%;
  overflow-y: scroll;
      justify-content: end;
}
}
</style>
