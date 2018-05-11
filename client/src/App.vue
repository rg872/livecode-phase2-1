<template>
  <div id="app">
    <div id="register-page" class="container d-flex justify-content-center" v-if="!isTokenExist">
      <div class="card" style="width: 18rem;">
        <div class="card-body">
          <div class="form-group">
            <label>Name</label>
            <input type="text" class="form-control" placeholder="Insert name" v-model="name">
          </div>
          <div class="form-group">
            <label>Email</label>
            <input type="email" class="form-control" placeholder="Inser email" v-model="email">
          </div>
          <button class="btn btn-primary" @click="signIn">Sign In</button>
        </div>
      </div>      
    </div>
    <div id="home-page" v-else>
        <router-view/>
      </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      isTokenExist : false,
      email: '',
      name: ''
    }
  },
  methods: {
    signIn () {
      axios.post('http://35.198.212.156/request-token', {
        name: this.name,
        email: this.email
      })
      .then(response => {
        console.log(response)
        localStorage.setItem('tokenUser', response.data.user.uid)
        this.isTokenExist = true
      })
      .catch(error => {
        console.log(error);
        
      })
    },
    checkToken () {
      if (localStorage.getItem('tokenUser')) {
        this.isTokenExist = true
      } else {  
        this.isTokenExist = false
      }
    }
  },

  created () {
     this.checkToken()
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
