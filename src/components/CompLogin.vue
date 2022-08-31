<template>
	<div>
    <h1>Login</h1>
    <div class="row mx-auto">
		<form class="mx-auto" style="max-width: 450px" action="https://m-fake-rest-api-nodejs.herokuapp.com/login" @submit.prevent="login()">
          <div class="alert alert-danger" v-if="error">
           {{error.response.data.message}}
          </div>
          <div class="form-group">
            <input v-model="loginUser.email" type="email" class="form-control" placeholder="Enter email">
          </div>
          <div class="form-group">
            <input v-model="loginUser.password" type="password" class="form-control" placeholder="Enter password">
          </div>
          <div class="form-group">
            <button style="width: 100%" role="button" class="btn btn-primary btn-block">Login</button>
          </div>
          <div class="form-group text-primary"><a class="pe-auto" href="#" @click="this.$router.push({name:'Register'});">Do not have an account ? Register here.</a></div>
        </form>
      </div>
	</div>
</template>

<script>

import LoginRequest from '../requests/LoginRequest'

export default{
	name : "CompLogin",
	data(){
		return{
			loginUser:{
				email:'',
				password:''
			},
			error:null,
		}
	},
  setup(){
    
  },
	mounted(){

	},
	computed:{

	},
	methods:{
		login:function(){
			LoginRequest.post('login',this.loginUser)
			.then( data => {
				console.log("login success !");
				window.localStorage.setItem('user',JSON.stringify(data));
				this.$router.push({name:'Post'}); 
				this.error = null ;
			})
			.catch( error => {
				this.error = error;
				console.log(error.message);
				console.log("login false !");
			})
		},
	}
}
</script>

<style scoped>
form {
  margin-bottom: 30px;
}
h1 {
	margin-top: 30px;
}
</style>