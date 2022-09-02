<template>
  <div id="big2">
    <h1>Add Post</h1>

    <div class="iput">
        <h3>Title</h3>
        <input v-model="post.title" placeholder="Title" class="form-control" aria-describedby="emailHelp" >
    </div>

    <div class="iput">
        <h3>Link Cover Photo</h3>
        <input v-model="post.link_img" placeholder="Link cover photo" class="form-control" aria-describedby="emailHelp" >
    </div>
    
    <div id="content">
        <h3>Content</h3>
        <span style="font-size:10px">Nếu không có công cụ chỉnh sửa Content vui lòng request để tải lại trang !</span>
        <quill-editor
            v-model:value="state.content"
            :options="state.editorOption"
            :disabled="state.disabled"
            @blur="onEditorBlur($event)"
            @focus="onEditorFocus($event)"
            @ready="onEditorReady($event)"
            @change="onEditorChange($event)"
        />
    </div>
    
    <div class="iput" id="stt">
        <h3>Status </h3>
        <input type="checkbox" v-model="post.status">
    </div>
    <hr><br>
    <div> <button class="btn btn-outline-primary btnsave" @click="savePost(post.id)">Save</button> </div>

  </div>
</template>

<script>

import { reactive } from "vue";
import BaseRequest from '@/core/BaseRequest';

export default {
    name : "PostEdit",
    setup() {
      const state = reactive({
        content: "<p></p>",
        _content: "",
        editorOption: {
          placeholder: "Content",
          modules: {

          },
        },
        disabled: false,
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
  
      return {
        state,
        onEditorBlur,
        onEditorFocus,
        onEditorReady,
        onEditorChange,
      };
    },
    data(){
        return{
            id_post:null,
            post:{
                id:null,
                title: '',
                content: '',
                read_number: null,
                link_img :'', 
                status: null
            }
        }
    },
    mounted(){
        this.id_post = window.localStorage.id_post;
        BaseRequest.get('posts/'+this.id_post)
        .then( data =>{
            this.post = data;
            this.state.content = this.post.content;
        }) 
        .catch(error=>{
            console.log(error.reponse.status);
        })

    },
    methods:{
        savePost:function(id_post){
            this.post.content = this.state.content;
            BaseRequest.put('posts/'+id_post,this.post)
            .then( () =>{
                alert("Chỉnh sửa bài viết thành công !");
                this.$router.push({name:'Post'}); 
                window.localStorage.removeItem('id_post');
            }) 
            .catch(error=>{
                console.log(error.reponse.status);
            })
        }
    },
    components:{

    }

}
</script>

<style scoped>
#big2 {
    padding: 60px;
    padding-top:30px;
}


.iput {
    display: flex;
    flex-wrap: wrap;
    justify-content: start;
    margin-bottom: 30px;
}
.iput input{
    font-weight: bold;
}
#title input {
    width: 100%px;
    font-weight: bold;
}

#content {
    margin-bottom: 30px;
}
#content h3 {
    margin-right: 1100px;
}

#stt h3 {
    margin-right: 10px;
}
.btnsave {
    transition: all 1s ease;
}
</style>