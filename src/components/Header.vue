<template>
    <div>
        <!-- icon font awesome -->
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.2.0/css/all.min.css" integrity="sha512-xh6O/CkQoPOWDdYTDqeRdPCVd1SpvCA9XXcUnZS2FmJNp1coAFzvtCN9BmamE+4aHK8yyUHUSCcJHgXloTyT2A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
        <!-- icon font awesome -->
        <div id="big">
            <div id="head">
                <div id="logo"><a @click="this.$router.push({name:'Post'});"><img src="./../assets/logo_blog.png" alt="HOME"></a></div>
                <div id="gr1" v-if="user==null">
                    <button class="btn btn-outline-primary" @click="this.$router.push({name:'Login'});">Login</button>
                    <button class="btn btn-outline-primary" @click="this.$router.push({name:'Register'});">Register</button>
                </div>
                <div v-if="user!=null">
                    <div class="dropdown">
                        <button class="btn btn-outline-primary new-post" @click="this.$router.push({name:'PostNew'});">New Post</button>
                        <button class="btn btn-secondary dropdown-toggle acc" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                            <i class="fa-solid fa-circle-user"></i>
                        </button>
                        <div class="dropdown-menu bt" aria-labelledby="dropdownMenuButton">
                            <a class="dropdown-item" href="#" style="font-weight:bold;overflow: hidden;" @click="this.$router.push({name:'InFor'});">User : {{name}}</a>
                            <a class="dropdown-item" href="#" @click="this.$router.push({name:'PostNew'});"><i class="fa-solid fa-plus"></i> New</a>
                            <a class="dropdown-item" href="#" @click="this.$router.push({name:'MyPost'});"><i class="fa-solid fa-list"></i> My Posts</a>
                            <a class="dropdown-item" href="#" @click="logout"><i class="fa-solid fa-arrow-right-from-bracket"></i> Logout</a>
                        </div>
                    </div>
                </div>
            </div>
            <div id="tit" v-if="title">
                <a  @click="this.$router.push({name:'Post'});"><i class="fa-solid fa-house"></i>Home</a>
                <i class="fa-solid fa-angle-right"></i>
                <a>{{title}}</a>
            </div>
        </div>
    </div>
</template>
  
<script>

import useEventBus from '../composables/useEventBus'

export default {
    name: 'CompHeader',
    data(){
        return {
            name:"",
            user:null,
            title:null,
        }
    },
    setup(){
		
    },
    mounted(){
        this.user = window.localStorage.getItem('user');
        this.name = window.localStorage.getItem('name');
        const { onEvent } = useEventBus()
        onEvent('geneEvent',()=>{
            if(window.localStorage.getItem('user')){
                this.user = window.localStorage.getItem('user');
                window.localStorage.setItem('name',JSON.parse(this.user).lastName + ' ' + JSON.parse(this.user).firstName);
                this.name = window.localStorage.getItem('name');
            }
		})

        onEvent('geneEvent_route',(tit)=>{
            this.title = tit;
		})
    },
    methods:{
        logout:function(){
            localStorage.clear();
            this.user = null;
            this.$router.push({name:'Login'});
        },
    },
    computed:{

    }
}
</script>
  
<style scoped>
#tit{
    width: 100%;
    height: 36px;
    border-top: 2px solid white;
    border-bottom: 2px solid white;
    text-align: start;
    padding-left: 6%;
    text-align: center;
    display: flex;
    align-items: center;
    line-height: 100%;
}
#tit a{
    cursor: pointer;
    font-weight: bold;
    transition: all 0.5s ease;
}
#tit > i{
    margin-left: 10px;
    margin-right: 10px;
}
#tit a i {
    margin-right: 10px;
}
#tit a:hover {
    color: #0085FF;
} 
.new-post {
    margin-left: 900px;
}
.acc {
    margin-left: 30px;
    width: 60px;
    height: 60px;
    padding: 0;
    border-radius: 30px;
    background-color: #0085FF;
    transition: all 1s ease;
    border: none;
}
.bt{
    margin-top: 10px;
    margin-right: 30px;
    width: 290px;
}
.bt a {
    color: #0085FF;
}
.bt a:hover {
    background-color: #0085FF;
    transition: all 1s ease;
    color: white;
}
.acc:hover {
    background: rgb(4, 116, 214) 0%;
    background: linear-gradient(0deg, rgb(4, 116, 214) 0%, rgba(17,92,226,0) 100%);
}
/* ẩn dấu tam giác của nút dropdown đi nó là cái affter */
.acc::after{
    display: none;
}
.acc i{
    font-size: 50px;
    color: white;
}
.btn {
    transition: all 1s ease;
}
#logo {
    top: 0px;
    margin-left: 5%;
}
#logo img {
    width: 60px;
    cursor: pointer;
    border-radius: 30px;
}
#logo img:hover{
    border: 3px solid #0085FF;
}
#head {
    width: 100%;
    height: 80px;
    text-align: start;
    align-content: center;
    align-items: center;
    display: flex;
    
}
#big {
    /* height: 118px; */
    background-color: #F6F6F6;
    background: rgb(139, 193, 239);
    background: linear-gradient(63deg, rgb(140, 191, 249) 0%, rgba(17,92,226,0) 100%);
    box-shadow: 0px 10px 10px -10px gray;
    border-top: 2px solid rgb(122, 183, 252);
}
#gr1 {
    margin-left: 900px;
}
#gr1 button {
    margin-left: 10px;
}
</style>
  