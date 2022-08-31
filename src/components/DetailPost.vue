<template>
  <div>
    <div id="post">
        <h1 class="alert alert-primary" role="alert">{{post.title}}</h1>
        <p id="content" class="alert alert-secondary" role="alert">{{post.content}}</p>
        <img :src="post.link_img" >
        <p>{{post.read_number}}</p>
        <p class="alert alert-success" role="alert" >Tác giả : {{post.auth}}</p>
        <!-- <p>ID_USER : {{post.id_user}}</p> -->
        Status <input type="checkbox" v-model="post.status" disabled>
        <div v-if="check"> 
            <button class="btn btn-primary" @click="editPost(post.id)">Edit</button> 
            <button class="btn btn-danger" @click="deletePost(post.id,post.title)" style="margin-left: 10px;">Delete</button> 
        </div>
        <br><hr><br>
    </div>
  </div>
</template>


<script scoped>

import BaseRequest from '@/core/BaseRequest';
import useEventBus from '../composables/useEventBus'

export default {
    name : "DetailPost",
    data(){
        return{
            post:{
                id:null,
                id_user:null,
                auth:'',
                title: '',
                content: '',
                read_number: null,
                link_img :'', 
                status: null
            },
            user:null,
            check:false,
        }
    },
    setup(){

    },
    computed(){

    },
    mounted(){
        const { emitEvent } = useEventBus()

        this.user = JSON.parse(window.localStorage.getItem('user'));
        let pathn =  window.location.pathname;
        this.axios.get('https://m-fake-rest-api-nodejs.herokuapp.com/posts?id='+pathn.slice(7,pathn.length))
        .then( response=>{
            this.post = response.data[0] ; 
            let title = this.post.title;
            if(this.user != null && this.post.id_user == this.user.id){
                this.check = true ;
            }

            emitEvent('geneEvent_route',title);
        }) 
        .catch(error=>{
            console.log(error.reponse.status);
        })
    },
    methods:{
        editPost:function(id_post){
            this.$router.push({name:'PostEdit'}); 
            window.localStorage.setItem('id_post',id_post);
        },
        deletePost:function(id_post,title){
            let text = "Bạn có chắc chắn muốn xóa bài viết : '"+title+"' !\nNhấn OK or Cancel.";
            if (confirm(text) == true) {
                BaseRequest.delete('posts/'+id_post)
                .then(()=>{
                    alert('Xóa bài viết : "'+title+'" thành công !');
                    this.$router.push({name:'Post'}); 
                })
                .catch(error => {
                    console.log(error.reponse.status)
                })
            } 
        },
        logout:function(){
			localStorage.clear();
			this.$router.push({name:'Login'});
		},
        // getPosts:function(pathn){
        //     // BaseRequest.get(pathn.substring(1,pathn.length))
        //     // console.log('posts?id='+pathn.slice(7,pathn.length));
            
        // }
    }
}
</script>

<style>
#post {
    margin: 30px;
}
#content {
    margin-left: 100px;
    margin-right: 100px;
}
#post img {
    width: 500px;
}
</style>