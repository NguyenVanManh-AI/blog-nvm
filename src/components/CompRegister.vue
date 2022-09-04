<template>
	<div>
		<h1>Register</h1>
		<div class="row mx-auto">
		<form class="col-7 mx-auto" action="https://m-fake-rest-api-nodejs.herokuapp.com/register" @submit.prevent="register()">
			<div class="alert alert-success" v-if="registerSucces">
				Đăng kí thành công !
			</div>
          <div class="text-danger" v-if="error"> 
            {{error.response.data.message}}  
            </div>
          <div class="form-group">
            <input v-model="registerUser.firstName" type="text" class="form-control" placeholder="First Name">
          </div>
          <div class="form-group">
            <input v-model="registerUser.lastName" type="text" class="form-control" placeholder="Last Name">
          </div>
          <div class="form-group">
            <input v-model="registerUser.userName" type="text" class="form-control" placeholder="Enter username">
          </div> 
          <div class="form-group">
            <input v-model="registerUser.email" type="email" class="form-control" placeholder="Enter email">
          </div> 
          <div class="form-group">
            <input v-model="registerUser.password" type="password" class="form-control" placeholder="Enter Password">
          </div> 
          <!-- <div class="form-group">
            <input v-model="registerUser.avatar" type="text" class="form-control" placeholder="Link avatar">
          </div>  -->
          <fieldset class="form-group">
              <div class="row" >
                <legend class="col-form-label col-sm-2 pt-0">Gender</legend>
                <div class="col-sm-2 d-flex" >
                  <div class="form-check">Male<input type="radio" name="gridRadios" v-model="registerUser.gender" value="Male"></div>
                  <div class="form-check">FeMale<input type="radio" name="gridRadios" v-model="registerUser.gender" value="FeMale"></div>
                </div>
              </div>
          </fieldset>
          <div class="form-group">
            <button style="width: 100%" role="button" class="btn btn-primary btn-block">Register</button>
          </div>
        </form>
        </div>
	</div>
</template>

<script>

import LoginRequest from '../requests/LoginRequest'

	export default{
    name : "CompRegister",
    created(){
        document.title = "Blog App - Register"
    },
		data(){
			return{ 
				registerUser:{
          firstName:'',
          lastName:'',
					userName:'',
          email: '',
          password: '',
          avatar:'',
          gender:''
				},
				error:null,
				registerSucces:false,
			}
		},
		methods:{
			register:function(){
				LoginRequest.post('register',this.registerUser) 
					.then( () =>{ 
						this.registerSucces = true;
            this.error=null;
            alert("Đăng kí thành công ! Chuyển đến trang Login để đăng nhập !");
            this.$router.push({name:"Login"});
					})
					.catch(error=>{ 
						this.error=error; 
						console.log(this.error)  
					})
			}
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