<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <input type="text" name="username" v-model="username"/> <br>
    <button @click="getUserData"> Cari Username Github </button>

    <div v-if="user !== null" class="profile">
      <img v-bind:src="user.avatar_url" width="200" height="200"> <br>
      <label>Username : {{ user.login }}</label> <br>
      <label>Id : {{ user.id }}</label> <br>
      <label>Profile name : {{ user.name }}</label> <br>
      <label>Profile bio : {{ user.bio }}</label> <br>
      <label>Puclic Repository : {{ user.public_repos }}</label> <br>
      <label>Profile URL : {{ user.url }}</label>
    </div>
      <info-card
      id="info-card"
      frontType="graph"
      frontTitle="Some Numbers"
      :frontData="[3, 2, 5, 9, 5, 10, 3, 5, 0, 0, 1, 8, 2, 9, 0]"
      backTitle="Quote of the Day"
      backData='<i>Do you know why people like violence? It is because it feels good. Humans find violence deeply satisfying. But remove the satisfaction, and the act becomes hollow.</i><br><span class="u-pull-right">— Alan Turing</span>'
    ></info-card>
  </div>
</template>

<script>
  import GithubService from '@/services/GithubService'
  import InfoCard from 'vue-info-card'

  export default {

    name: 'hello',
    data () {
      return {
        msg: 'Vue PWA + Github API',
        username: '',
        user: null
      }
    },

    methods: {
      async getUserData() {
        this.user = null;

        const result = await GithubService.searchUser({
          username: this.username
        }).then(response => {
          this.user = response.data
        }).catch(error => {
          console.log(error)
        })

        if(this.user !== null) {
          console.log(this.user);
        }
      }
    }
  }
</script>

<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  input {
    width: 40%;
    padding: 12px 20px;
    margin: 8px 0; 
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }


  button {
    background-color: #4CAF50;
    border: none;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
  }

  .profile {
    padding: 16px;
  }

  label {
    font-size: 20px;
  }
  
</style>
