<template>
  <div>
    <div class="article">
      <div v-for="(postt,index) in list_post" :key="index" @click="showMore(postt.id)">
        <p class="back-gr"></p>
        <div><img :src="postt.link_img" ></div>
        <h3>{{postt.title}}</h3>
        <div style="width:100%;cursor: pointer;">
          <quill-editor
            v-model:value="postt.content"
            :options="state.editorOption"
            :disabled="state.disabled"
            @change="onEditorChange($event)"
            />
        </div>
        <p>{{postt.read_number}}</p>
        <!-- <p>ID_USER : {{postt.id_user}}</p> -->
        <p>Id Post : {{postt.id}}</p>
        Status <input type="checkbox" v-model="postt.status" disabled>
        <br><hr><br>
      </div>
    </div>
    <h1 v-if="this.number_post<=0">Bạn chưa có bài viết nào . Hãy đi tạo mới bài viết .</h1>
    
    <paginate class="pag"
      :page-count="Math.ceil(this.number_post/6)"
      :page-range="3"
      :margin-pages="2"
      :click-handler="clickCallback"
      :prev-text="'Prev'"
      :next-text="'Next'"
      :container-class="'pagination'"
      :page-class="'page-item'">
    </paginate>
  </div>
</template>


<script scoped>

import BaseRequest from '@/core/BaseRequest';
import Paginate from 'vuejs-paginate-next';
import { reactive } from "vue";

export default {
    name : "CompPost",
    created(){
        document.title = "Blog App - My Posts"
    },
    components:{
        paginate: Paginate
    },
    data(){
        return{
            posts:null,
            user:null,
            number_post:null,
            myPosts:[],
            list_post:null,
        }
    },
    setup(){
      // rich
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
      // rich

      return {
        // rich
        state,
        onEditorBlur,
        onEditorFocus,
        onEditorReady,
        onEditorChange,
      }
    },
    computed(){
        console.log(this.posts);
    },
    mounted(){
        this.user = window.localStorage.getItem('user');
        this.getPosts();
    },
    methods:{
        editPost:function(id_post){
            console.log(id_post);
            window.location="https://blog-nvm.herokuapp.com/post/edit"
            window.localStorage.setItem('id_post',id_post);
        },
        deletePost:function(id_post,title){
            let text = "Bạn có chắc chắn muốn xóa bài viết : '"+title+"' !\nNhấn OK or Cancel.";
            if (confirm(text) == true) {
                console.log(id_post);
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
        getPosts:function(){
            BaseRequest.get('posts')
                .then( data=>{
                    this.posts = data.reverse() ; 
                    this.filt(this.posts);
                }) 
                .catch(error=>{
                    console.log(error.reponse.status);
                })
        },
        filt:function(posts){
            for(let i=0;i<posts.length;i++){
                if(posts[i].id_user == JSON.parse(this.user).id){
                    this.myPosts.push(posts[i]);
                }
            }
            this.number_post = this.myPosts.length;
            this.list_post = this.myPosts.slice(0, 6);
        },
        clickCallback : function(pageNum){
            if(pageNum == 1){
                this.list_post = this.myPosts.slice(0, 6);
            }
            else {
                this.list_post = this.myPosts.slice((pageNum-1)*6, 6*pageNum);
            }
        },
        showMore:function(id_post){
            this.$router.push({name:'DetailPost',params:{id:id_post}}); 
        }
    }
}
</script>

<style scoped>
.pag {
  margin-left:41%;
  margin-bottom: 60px;
}
.article{
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  padding: 20px;
}
.article h3 {
  margin-top: 10px; /* font */
  font-family: 'Lobster', cursive;
}
.article > div {
  width: 30%;
  border-top: 6px solid white;
  border-left: 6px solid white;
  box-shadow: 25px 25px 10px -10px rgb(210, 210, 210);
  border-radius:20px ;
  height: 360px;
  margin: 10px;
  overflow: hidden;
  position: relative;
}
.article > div > div {
  width: 100%;
  overflow: hidden;
}
.article div img {
  width: 100%;
  transition: all .3s ease;
}
.article div:hover {
  cursor: pointer;
}
.article div:hover img {
  transform: scale(1.3);
}
.article div:hover > .back-gr{
  background: linear-gradient(0deg, rgb(174, 250, 172) 0%, rgba(245,177,33,0) 100%);
}
.back-gr{
  position:absolute;
  bottom:-20px;
  left:0px;
  background: linear-gradient(0deg, rgb(97, 247, 120) 0%, rgba(245,177,33,0) 100%);
  width: 100%;
  height: 60%;
}
</style>