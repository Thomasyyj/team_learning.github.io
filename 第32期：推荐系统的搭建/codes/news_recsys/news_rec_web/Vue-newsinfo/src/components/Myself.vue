<template>
  <div class="my-info">

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css" />

    <div class="profile">
      <div class="profile-pic">
        <div class="header-color"></div>
        <img class="tx" src="../images/datawhale.png" alt="头像">
      </div>
      <div class="title">
        <h1>{{username}}</h1>
      </div>
      <button class="follow" @click="quit">退出登录</button>
      <div class="description">
        <h4 class="about">DataWhale 新闻推荐开源项目</h4>
        <p class="datawhale">Datawhale是一个专注于数据科学与AI领域的开源组织，汇集了众多领域院校和知名企业的优秀学习者，
          聚合了一群有开源精神和探索精神的团队成员。Datawhale 以“for the learner，和学习者一起成长”为愿景，
          鼓励真实地展现自我、开放包容、互信互助、敢于试错和勇于担当。同时 Datawhale 用开源的理念去探
          索开源内容、开源学习和开源方案，赋能人才培养，助力人才成长，建立起人与人，人与知识，人与企业和
          人与未来的联结。</p>
        <img src="../images/dw.png" alt="二维码" class="dwimg">
      </div>
      <bottomBarVue></bottomBarVue>
    </div>
  </div>
</template>
<style scoped>
  .content {
    margin-top: 40px;
  }

  .mint-cell {
    min-height: 40px;
  }

  span {
    font-size: 13px;
  }
</style>
<script>
  import bottomBarVue from "./bottomBar.vue"
import common from './common.vue'
  export default {
    data() {
      return {
        username: 'user'
      }
    },
    components: {
      bottomBarVue
    },
    methods: {
      quit() {
        this.$store.dispatch('deleteCacheView', 'recLists')
        this.$store.dispatch('deleteCacheView', 'hotLists')
        /*删除cookie*/
        this.cookie.clearCookie('LoginName')
        this.cookie.clearCookie('openId')
        this.$router.push('/signIn')
        }
    },
    created() {
      // this.username = localStorage.username
      this,
      this.username = this.cookie.getCookie('LoginName')
    }
  }
</script>
<style scoped>
  @import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;600;700&display=swap");

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  html,
  body {
    height: 100vh;
    display: grid;
    background: #ede1e7;
    font-family: "Open Sans", sans-serif;
  }

  .profile {
    margin: auto;
    /* height: 710px;
    width: 400px; */
    background: #ffffff;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    /* border-radius: 20px; */
    box-shadow: rgba(0, 0, 0, 0.19) 0px 10px 20px, rgba(0, 0, 0, 0.23) 0px 6px 6px;
    -ms-overflow-style: none;
    /* for Internet Explorer, Edge */
    scrollbar-width: none;
    /* for Firefox */
    overflow-y: scroll;
  }

  ::-webkit-scrollbar {
    display: none;
    /* for Chrome, Safari, and Opera */
  }

  .header-color {
    /* border-radius: 20px 20px 0 0; */
    padding-bottom: 150px;
    width: 100vh;
    background: #4f759b;
  }

  .profile-pic img {
    height: 40vw;
    width: 40vw;
    border-radius: 50%;
    border: 10px solid #ffffff;
    margin-top: -100px;
  }

  .title {
    margin-bottom: 25px;
  }

  h1 {
    font-size: 32px;
    font-weight: 700;
    color: #131b23;
    margin-bottom: 10px;
    letter-spacing: 0.025em;
  }

  .description {
    margin-bottom: 25px;
    color: #131b23;
    letter-spacing: 0.01em;
  }

  .description p:not(:last-child) {
    margin-bottom: 5px;
  }

  button {
    font-family: "Open Sans", sans-serif;
    color: #ffffff;
    background: #4f759b;
    font-size: 18px;
    font-weight: 600;
    letter-spacing: 0.025em;
    border: none;
    border-radius: 15px;
    min-height: 35px;
    width: 100px;
    margin-bottom: 25px;
    transition: all 0.2s ease;
    cursor: pointer;
  }

  button:hover {
    width: 115px;
    background: #4f759be0;
  }

  .about {
    /* display: flex;
    justify-content: space-between; */
    padding-top: 20px;
    padding-bottom: 20px;
    text-align: center;
    font-weight: bold;
  }

  .datawhale {
    font-size: 1.3rem;
    padding: 0 20px 20px 20px;
    text-indent: 2em;
    line-height: 2.4rem;
  }

  .dwimg {
    width: 150px;
    height: 150px;
    display: block;
    margin: 15px auto 100px auto;
  }
</style>