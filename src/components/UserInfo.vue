<template>
<div class="UserInfo">
  <div class="loading" v-if="isLoading">
    <img src="../assets/loading.gif">
  </div>
  <div class="userInfomation" v-else>
    <section>
      <p>主页</p>
      <img :src="userinfo.avatar_url">
      <span>{{userinfo.loginname}}</span>
      <p>
        {{userinfo.score}}积分
      </p>
      <p>
        注册时间：{{userinfo.create_at | formatDate}}
      </p>
    </section>
    <div class="replies">
      <p>最近回复的话题</p>
      <ul>
        <li v-for="item in userinfo.recent_replies">
          <router-link :to="{
              name:'post_content',
              params:{
                  id:item.id
              }
          }">
          <!--这里的文章详情页打开只传递了一个参数：文章id，也可以打开页面(但实际打开的页面的路由仍有两个参数)-->
            {{item.title}}
          </router-link>
        </li>
      </ul>
    </div>
    <div class="topics">
      <p>最近创建的话题</p>
      <ul>
        <li v-for="item in userinfo.recent_topics">
          <router-link :to="{
              name:'post_content',
              params:{
                  id:item.id
              }
          }">
            {{item.title}}
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</div>
</template>

<script>
export default{
    name:'UserInfo',
    data(){
        return{
            isLoading:false,
            userinfo:{}
        }
    },
    methods:{
        getData(){
            this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
            //这个路径跟index.js中的路径不太一样？
            //因为这个路径是cnode社区API提供的，不属于我们的路由
            //我们的路由请求的路径是我们项目中用的，获取的参数是API提供的
            //另外，注意，this.$route.params.name利用了我们路由中的params-name
            .then(res=>{
              this.isLoading=false;
              this.userinfo=res.data.data;
            })
            .catch(err=>{
              console.log(err)
            })
        }
    },
    beforeMount(){
        this.isLoading=true;
        this.getData();
    }
}
</script>

<style scoped>
  .userInfomation {
    background: white;
    width: 75%;
    margin: 10px auto;
  }
  .userInfomation section {
    padding: 12px;
  }
  .userInfomation img {
    width: 30px;
  }
  .userInfomation li {
    list-style:none;
  }
  .userInfomation .replies,
  .userInfomation .topics {
    font-size: 0.72rem;
    border-top: 10px #DDDDDD solid;
  }
  .userInfomation > div > p {
    padding: 12px 0 12px 12px;
    background-color: rgba(212, 205, 205, 0.17);
    font-size: 0.75rem;
    margin: 0;
  }
  .userInfomation > div >ul > li {
    padding: 4px 0 4px 12px;
    white-space: nowrap;
    font-size: 0.72rem;
    text-overflow: ellipsis;
    overflow: hidden;
    line-height: 30px;
    vertical-align: middle;
  }
  .userInfomation > div >ul > li > a {
    color: #094E99;
    text-decoration: none;
  }

</style>
