<template>
  <div id="big">
    <h1>Add Post</h1>

    <div class="iput">
        <h3>Title</h3>
        <input v-model="post.title" placeholder="Title" class="form-control" aria-describedby="emailHelp" >
    </div>

    <div class="iput">
        <h3>Link Cover Photo</h3>
        <input v-model="post.link_img" placeholder="Link cover photo" class="form-control" aria-describedby="emailHelp" >
    </div>
    
    <div id="h3content">
        <h3 >Content</h3>
    </div>
    <span style="font-size:10px">Nếu không có công cụ chỉnh sửa Content vui lòng request để tải lại trang !</span>
    <p style="font-size:10px">Bạn có thể copy paste một bài viết bao gồm ảnh , text ,... của người khác or web khác rồi cho vào content để làm nội dung của bạn . Hoặc có thể import ảnh từ chính máy của bạn và Import Video ở trang web khác .</p>
    <div id="content">
        <quill-editor
            v-model:value="state.content"
            :options="state.editorOption"
            :disabled="state.disabled"
        />
    </div>
    
    <div class="iput" id="stt">
        <h3>Status </h3>
        <input type="checkbox" v-model="post.status">
    </div>
    <hr>
    <div> <button class="btn btn-outline-primary btnadd" @click="addPost()">Add</button> </div>

</div>
</template>

<script>

import { reactive } from "vue";
import BaseRequest from '@/core/BaseRequest';

export default {
    
    name : "PostNew",
    components:{
        
    },
    created(){
        document.title = "Blog App - New"
    },
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
  
    //   const onEditorBlur = (quill) => {
    //     console.log("editor blur!", quill);
    //   };
    //   const onEditorFocus = (quill) => {
    //     console.log("editor focus!", quill);
    //   };
    //   const onEditorReady = (quill) => {
    //     console.log("editor ready!", quill);
    //   };
    //   const onEditorChange = ({ quill, html, text }) => {
    //     console.log("editor change!", quill, html, text);
    //     state._content = html;
    //   };
  
      return {
        state,
        // onEditorBlur,
        // onEditorFocus,
        // onEditorReady,
        // onEditorChange,
      };
    },
    data(){
        return{
            id_post:null,
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
        }
    },
    mounted(){

    },
    methods:{
        addPost:function(){
            this.post.content = this.state.content;
            this.post.id_user = JSON.parse(window.localStorage.getItem('user')).id;
            this.post.auth = JSON.parse(window.localStorage.getItem('user')).lastName + 
                            JSON.parse(window.localStorage.getItem('user')).firstName;
            BaseRequest.post('posts',this.post)
            .then(()=>{
                alert("Thêm bài viết thành công !");
				this.$router.push({name:'Post'}); 
            }) 
            .catch(error=>{
                console.log(error.reponse.status);
            })
        },
    }

}
</script>

<style scoped>
#big {
    padding-top: 30px;
    padding-left: 60px;
    padding-right: 60px;
    padding-bottom: 60px;
    width: 100%;
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
    width: 100%;
}
#h3content {
    display: flex;
    flex-wrap: wrap;
    justify-content: start;
}

#stt h3 {
    margin-right: 10px;
}
.btnadd {
    transition: all 1s ease;
}
</style>