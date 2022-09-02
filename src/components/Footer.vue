<template>
    <div>
        <div id="footer">
          <div id="logo"><a href="https://blog-nvm.herokuapp.com/"><img src="./../../public/logo_blog.png" alt="HOME"> BLOG APP</a></div>
          <div id="top">
            <ul>
              <li class="n1">
                <ul>
                  <li><i class="fa-solid fa-code"></i> SKILL</li>
                  <li><a href="#">Html</a></li>
                  <li><a href="#">Css</a></li>
                  <li><a href="#">Javascript</a></li>
                  <li><a href="#">PHP</a></li>
                  <li><a href="#">Mysql</a></li>
                </ul>
              </li>
              <li class="n1">
                <ul>
                  <li><i class="fa-solid fa-check"></i> FRAMEWORK</li>
                  <li><a href="#">Bootstrap</a></li>
                  <li><a href="#">Jquery</a></li>
                  <li><a href="#">VueJs</a></li>
                  <li><a href="#">Bootstrap Vue</a></li>
                  <li><a href="#">Laravel</a></li>
                </ul>
              </li>
              <li class="n1">
                <ul>
                  <li><i class="fa-sharp fa-solid fa-toolbox"></i> TOOL</li>
                  <li><a href="#">Vs Code</a></li>
                  <li><a href="#">Sublime Text</a></li>
                  <li><a href="#">Docker</a></li>
                  <li><a href="#">Xampp</a></li>
                  <li><a href="#">PhpMyadmin</a></li>
                  <li><a href="#">Postman</a></li>
                  <li><a href="#">NPM</a></li>
                  <li><a href="#">Composer</a></li>
                </ul>
              </li>
              <li class="n1">
                <ul>
                  <li><i class="fa-solid fa-diagram-project"></i> PROJECT</li>
                  <li><a href="#">Modest</a></li>
                  <li><a href="#">Gohub</a></li>
                  <li><a href="#">Youtube</a></li>
                  <li><a href="#">Blog App</a></li>
                  <li><a href="#">Manager Sales</a></li>
                  <li><a href="#">Others</a></li>
                </ul>
              </li>
              <li class="n2">
                <ul>
                  <li><i class="fa-solid fa-heart"></i> FOLLOW ME</li>
                  <li id="aboutme"><a href="#" @click="this.$router.push({name:'AboutMe'});"><i class="fa-solid fa-user-shield"></i> About Me</a></li>
                  <li id="facebook"><a target="_blank" href="https://www.facebook.com/IcassNiuTon"><i class="fa-brands fa-facebook"></i> Facebook</a></li>
                  <li id="instagram"><a target="_blank" href="https://www.instagram.com/vanmanh_ai_machine_learning/"><i class="fa-brands fa-instagram"></i> Instagram</a></li>
                  <li id="twitter"><a target="_blank" href="https://twitter.com/NguyenVanManhft"><i class="fa-brands fa-twitter"></i> Twitter</a></li>
                  <li id="github"><a target="_blank" href="https://github.com/NguyenVanManh-AI"><i class="fa-brands fa-github"></i> Github</a></li>
                  <li id="linkedin"><a target="_blank" href="https://www.linkedin.com/in/nvmanhfullstack/"><i class="fa-brands fa-linkedin"></i> Linkedin</a></li>
                </ul>
              </li>
            </ul>
          </div>
          <span></span>
          <div id="bottom">
            <label> © 2022 Blog App, Inc.</label>
            <label>@Nguyen Van Manh | FullStack Developer</label>
          </div>
        </div>
    </div>
</template>
  
<script>
  export default {
    name: 'CompFooter',
  }
</script>

<style scoped>
#footer {
  width: 100%;
  border: 1px solid transparent;
  background-color: #0e2d52;
  color: white;
  text-align: start;
  padding-right: 210px;
  padding-left: 260px;
}
#footer ul,li {
  list-style: none;
  margin: 0;
  padding: 0;
}
.n1 a {
  text-decoration: none;
  transition: all 1s ease;
  color: #d7d7d7;
}
.n2 a {
  text-decoration: none;
  transition: all 1s ease;
  color: #d7d7d7;
}
#logo {
  margin-top: 60px;
}
#logo img {
    width: 50px;
    cursor: pointer;
    border-radius: 25px;
    margin-right: 5px;
}
#logo a {
  cursor: pointer;
  background: -webkit-linear-gradient(63deg,#012adf, rgb(255, 255, 255));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: bold;
  letter-spacing: 4px;
  /* word-spacing: 10px;// giữa các từ  */
}

#top {
  margin-top: 25px;
  margin-bottom: 60px;
}
#top ul,li {
  cursor: pointer;
  transition: all 0.5s ease ;
}
#top > ul {
  display: flex;
}
#top > ul > li {
  margin-right: 73px;
}
#top > ul > li > ul > li:nth-child(1){
  font-size: 14px;
  margin-bottom: 10px;
  background: -webkit-linear-gradient(#0489d7, #012adf);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: bold;
}

.n1 > ul > li > a:hover{
  color: red;
}

#aboutme a:hover {
  color: green;
}
#facebook a:hover{
  color: #1877F2;
}
#twitter a:hover{
  color: #37B9FF;
}
#github a:hover{
  color: black;
}
#linkedin a:hover{
  color: #0A66C2;
}
#instagram a:hover{
  color: rgb(255, 174, 0);
}
#top > ul > li > ul > li:not(#top > ul > li > ul > li:nth-child(1)){
  font-size: 13px;
  margin-bottom: 2px;
}

#footer span {
  display: block;
  width: 100%;
  height: 3px;
  background-color: white;
  margin-bottom: 20px;
}
#bottom {
  margin-bottom:20px;
  font-size: 13px;
  color: gray;
  justify-content:space-between;
  display: flex;
}

</style>
