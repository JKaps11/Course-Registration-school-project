<template>
  <main> 
    <div class = "left">
      <img src ="../assets/images/logo5.png" alt="HusckyLogo" width="320" height="400">
    </div>

    <div class = "middle">
      <h2 class = "title">{{versionState.getLoginTitle.value}}</h2>
      <form name="login-form">

        <!-- Could add hover to input for clearner look down the road --->

        <div>
          <label for="username" class = "upText">Username</label>
          <input class = "upFormat" type="text" id="username" v-model="username" />
        </div>
        <div>
          <label for="password" class = "upText">Password</label>
          <input class = "upFormat" type="password" id="password" v-model="password" />
        </div>
        <button class="btn" type="submit" v-on:click.prevent="login()">
          Login
        </button>
      </form>
    </div>

   <div class = "right">
     <img src ="../assets/images/logo5.png" alt="HusckyLogo" width="320" height="400">
   </div>
  </main>
</template>

<script setup>
import {ref} from 'vue';
import versionState from '../state/version';
import {useRouter} from 'vue-router';

const router = useRouter();
const apiURL = 'api url for previous login authentication before auth0'

const username = ref('');
const password = ref('');
let state = ''

//checks if user has a username and password in our database
//Will replace with auth 0
const login = () => {
  
    //construct the url of the get request

    if(versionState.getVersion.value){
      state = 'user'
    }else{
      state = 'teacher'
    }
    const url = `${apiURL}?username=${username.value}&password=${password.value}&state=${state}`
    fetch(url, {
      method: 'GET',
    })
    .then(response => {
      console.log(response.status)
    if (response.ok) {
      console.log('Request was successful');
      router.push('/home');
    }
    if (response.status == 400){
      alert('Please input a username and password');
      router.push('/login');
    }
    if (response.status == 401){
      alert('Username or password are incorrect. Please try again.');
      router.push('/login');
    }
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });
  }

</script>

<style>

main {
  display: flex;
  flex-direction:row;  
}

img {
  display: block;
  margin-top: 50px;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: auto;
}

.middle {
  margin-top: 35px;
  width: 34%;
  margin-bottom: 35px;
  background-color: #151E3D;
}

.title {
  text-align: center;
  margin-bottom: 25px;
  font-size: 30px;
  font-weight: bold;
  background-color: #151E3D;
  color: white;
  padding: 25px;
}

.upFormat{
  margin-left: auto;
  margin-right: auto;
  font-size: 25px;
  display: block;
}

.upText {
  padding: 10px;
  margin-left: auto;
  margin-right: auto;
  display: block;
  color: White;
  font-weight: semi-bold;
  font-size: 25px;
  text-align: center;
}

.btn{
  margin-top: 20px;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 100px;
  display: block;
  color: Black;
  background-color: White;
  border: 2px solid Black;
  font-weight: bold;
  font-size: 20px; 
  padding: 10px 40px;
}

.btn:hover {
  background-color: Gray;
}

.loginButtonWriting {
  font-weight: bold;
  font-size: 20px; 
  padding: 15px 40px;
}

.left {
  width: 33%;
}

.right {
  width: 33%;
}

</style>
