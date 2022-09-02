<template>
  <div>
    <div id="post">
        <h1 class="alert alert-primary" role="alert">{{post.title}}</h1>
        <img :src="post.link_img" >
        <!-- <p id="content" class="alert alert-secondary" role="alert" v-html="post.content"></p> -->

        <div  class="vueq">
            <quill-editor
            v-model:value="state.content"
            :options="state.editorOption"
            :disabled="state.disabled"
            @change="onEditorChange($event)"
            />
        </div>

        
        <p>{{post.read_number}}</p>
        <p class="alert alert-success" role="alert" >Tác giả : {{post.auth}}</p>
        <!-- <p>ID_USER : {{post.id_user}}</p> -->
        Status <input type="checkbox" v-model="post.status" disabled>
        <div v-if="check"> 
            <button class="btn btn-outline-primary btngr" @click="editPost(post.id)">Edit</button> 
            <button class="btn btn-outline-danger btngr" @click="deletePost(post.id,post.title)" style="margin-left: 10px;">Delete</button> 
        </div>
        <br><hr><br>
    </div>
  </div>
</template>


<script scoped>

import BaseRequest from '@/core/BaseRequest';
import useEventBus from '../composables/useEventBus'
import router from './../router/routes' 
import { reactive } from "vue";

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
    setup() {
      const state = reactive({
        content: "<p></p>",
        _content: "",
        editorOption: {
          placeholder: "core",
          modules: {
            toolbar: [  ],
            // other moudle options here
            // otherMoudle: {}
          },
          // more options
        },
        disabled: true,
      });
  
      const onEditorBlur = (quill) => {
        console.log("editor blur!", quill);
      };
      const onEditorFocus = (quill) => {
        console.log("editor focus!", quill);
      };
      const onEditorReady = (quill) => {
        console.log("editor ready!", quill);
      };
      const onEditorChange = ({ quill, html, text }) => {
        console.log("editor change!", quill, html, text);
        state._content = html;
      };
  
      // setTimeout(() => {
      //   state.disabled = true;
      // }, 2000);
  
      return {
        state,
        onEditorBlur,
        onEditorFocus,
        onEditorReady,
        onEditorChange,
      };
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
            this.state.content = this.post.content;
            if(this.user != null && this.post.id_user == this.user.id){
                this.check = true ;
            }

            emitEvent('geneEvent_route',title);
        }) 
        .catch(()=>{
            router.push({name:'NotFound'});
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

<style scoped>
.btngr{
    transition: all 1s ease;
}
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
.vueq {
    width: 1200px;
}
</style>