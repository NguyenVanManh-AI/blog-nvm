<template>
    <div>
        <h1>Information User</h1>
        <div class="row mx-auto">
            <form class="col-6 mx-auto" @submit.prevent="saveInfor(this.user.id)">
                <div class="alert alert-success" v-if="err">
                    Đăng kí thành công !
                </div>
                <div class="text-danger" v-if="error"> 
                    {{error.response.data.message}}  
                    </div>
                <div class="form-group">
                    <input v-model="user.firstName" type="text" class="form-control bg-light font-weight-bold" placeholder="first name">
                </div>
                <div class="form-group">
                    <input v-model="user.lastName" type="text" class="form-control bg-light font-weight-bold" placeholder="last name">
                </div>
                <div class="form-group">
                    <input v-model="user.userName" type="text" class="form-control bg-light font-weight-bold" placeholder="username">
                </div> 
                <div class="form-group">
                    <input v-model="user.email" disabled type="email" class="form-control bg-light font-weight-bold" placeholder="email">
                </div> 
                <fieldset class="form-group">
                    <div class="row" >
                        <legend class="col-form-label col-sm-2 pt-0">Gender</legend>
                        <div class="col-sm-2 d-flex" >
                        <div class="form-check bg-light font-weight-bold">Male<input type="radio" name="gridRadios" v-model="user.gender" value="Male"></div>
                        <div class="form-check bg-light font-weight-bold">FeMale<input type="radio" name="gridRadios" v-model="user.gender" value="FeMale"></div>
                        </div>
                    </div>
                </fieldset>
                <div class="form-group">
                    <button style="width: 100%" role="button" class="btn btn-primary btn-block">Save</button>
                </div>
            </form>
        </div>
    </div>
</template>


<script scoped>

import BaseRequest from '@/core/BaseRequest';
import useEventBus from '../composables/useEventBus'

export default {
    name : "InFor",
    created(){
        document.title = "Blog App - Information"
    },
    data(){
        return{
            user:{
                id:null,
                firstName:'',
                lastName:'',
                userName:'',
                email: '',
                avatar:'',
                gender:'',
                accessToken:'',
                refreshToken:'',
                createdAt:null,
                modifiedAt:null,               
            },
            err:null,
        }
    },
    setup(){
      const { emitEvent } = useEventBus()
      return {
        geneEvent:function(){
          emitEvent('geneEvent');
        },
      }
    },
    computed(){

    },
    mounted(){
        this.user = JSON.parse(window.localStorage.getItem('user'));
    },
    methods:{
        saveInfor:function(id_user){
            if(this.user.lastName == '' && this.user.firstName == ''){
                alert("Không được để trống First Name hoặc Last Name !");
            }
            else {
                let s = JSON.stringify(this.user);
                BaseRequest.patch('users/'+id_user,this.user)
                .then( () =>{
                    alert("Chỉnh sửa thông tin thành công !");
                    window.localStorage.setItem('user',s);
                    this.geneEvent();
                    this.err = null;
                }) 
                .catch(error=>{
                    console.log(error.reponse.status);
                    this.err = error.reponse.status;
                })
            }
            
        }
    }
}
</script>

<style scoped>
h1 {
    margin-top:30px
}
</style>