<template>
  <div>
    <div class="article">
      <div v-for="(postt,index) in list_post" :key="index" @click="showMore(postt.id);">
        <p class="back-gr"></p>
        <div><img :src="postt.link_img" ></div>
        <h3>{{postt.title}}</h3>
        <p>Tác giả : {{postt.auth}}</p>
        <p>{{postt.content}}</p>
        <p>{{postt.read_number}}</p>
        <!-- <p>ID_USER : {{postt.id_user}}</p> -->
        <p>Id Post : {{postt.id}}</p>
        Status <input type="checkbox" v-model="postt.status" disabled>
        <br><hr><br>
      </div>
    </div>
    
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

import Paginate from 'vuejs-paginate-next';
import BaseRequest from '@/core/BaseRequest';
import useEventBus from '../composables/useEventBus'

export default {
    name : "CompPost",
    components:{
        paginate: Paginate
    },
    data(){
        return{
            posts:null,
            number_post:null,
            list_post:null,
            show:true,
        }
    },
    computed:{

    },
    setup(){
      const { emitEvent } = useEventBus()
      return {
        geneEvent:function(){
          emitEvent('geneEvent');
        },
      }
    },
    mounted(){
        this.geneEvent();
        BaseRequest.get('posts')
        .then( data =>{
            this.posts = data.reverse() ; 
            this.number_post = this.posts.length;
            this.list_post = this.posts.slice(0, 6);
        }) 
        .catch(error=>{
            console.log(error.reponse.status);
        })
    },
    methods:{
      clickCallback : function(pageNum){
        if(pageNum == 1){
          this.list_post = this.posts.slice(0, 6);
        }
        else {
          this.list_post = this.posts.slice((pageNum-1)*6, 6*pageNum);
        }
      },
      showMore:function(id_post){
				this.$router.push({name:'DetailPost',params:{id:id_post}}); 
      }
    }

}
</script>

<style scoped>

/* google font */
@import url('https://fonts.googleapis.com/css2?family=Lobster&display=swap');/* font */

* {
  list-style: none;
}

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
  box-shadow: 20px 20px 10px -10px gray;
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
  background: linear-gradient(0deg, rgb(149, 202, 251) 0%, rgba(245,177,33,0) 100%);
}
.back-gr{
  position:absolute;
  bottom:-20px;
  left:0px;
  background: linear-gradient(0deg, rgb(94 168 239) 0%, rgba(245,177,33,0) 100%);
  width: 100%;
  height: 60%;
}
</style>