<template>
  <div>
    <div class="tabs">
      <input type="radio" id="tab1" name="tab-control">
      <input type="radio" id="tab2" name="tab-control" checked>
      <ul>
        <li title="Features" @click="toRec">
          <label for="tab1" role="button">
            <svg viewBox="0 0 24 24">
              <path d="M14,2A8,8 0 0,0 6,10A8,8 0 0,0 14,18A8,8 0 0,0 22,10H20C20,13.32 17.32,16 14,16A6,6 0 0,1 8,10A6,6 0 0,1 14,4C14.43,4 14.86,4.05 15.27,4.14L16.88,2.54C15.96,2.18 15,2 14,2M20.59,3.58L14,10.17L11.62,7.79L10.21,9.21L14,13L22,5M4.93,5.82C3.08,7.34 2,9.61 2,12A8,8 0 0,0 10,20C10.64,20 11.27,19.92 11.88,19.77C10.12,19.38 8.5,18.5 7.17,17.29C5.22,16.25 4,14.21 4,12C4,11.7 4.03,11.41 4.07,11.11C4.03,10.74 4,10.37 4,10C4,8.56 4.32,7.13 4.93,5.82Z" />
            </svg>
            <br>
            <span>推荐</span>
          </label>
        </li>
        <li title="Delivery Contents" class="hot" @click="toHot">
          <label for="tab2" role="button" class="hotCont">
            <svg viewBox="0 0 24 24" class="hotPath">
              <path d="M2,10.96C1.5,10.68 1.35,10.07 1.63,9.59L3.13,7C3.24,6.8 3.41,6.66 3.6,6.58L11.43,2.18C11.59,2.06 11.79,2 12,2C12.21,2 12.41,2.06 12.57,2.18L20.47,6.62C20.66,6.72 20.82,6.88 20.91,7.08L22.36,9.6C22.64,10.08 22.47,10.69 22,10.96L21,11.54V16.5C21,16.88 20.79,17.21 20.47,17.38L12.57,21.82C12.41,21.94 12.21,22 12,22C11.79,22 11.59,21.94 11.43,21.82L3.53,17.38C3.21,17.21 3,16.88 3,16.5V10.96C2.7,11.13 2.32,11.14 2,10.96M12,4.15V4.15L12,10.85V10.85L17.96,7.5L12,4.15M5,15.91L11,19.29V12.58L5,9.21V15.91M19,15.91V12.69L14,15.59C13.67,15.77 13.3,15.76 13,15.6V19.29L19,15.91M13.85,13.36L20.13,9.73L19.55,8.72L13.27,12.35L13.85,13.36Z" />
            </svg>
            <br>
            <span>热门</span>
          </label>
        </li>
      </ul>
    </div>
    <div class="lists">
      <van-list v-model="vanListLoading" :finished="finished" :finished-text="finishedText" @load="onLoad" :offset=300>
        <van-cell v-for="(item,index) in hotContent" :key="item.news_id">
          <!-- 路由地址传参,需要前面加：号，表示这个参数不是字符串 -->
          <router-link :to="{name:'NewsInfo' ,params:{id:item.news_id,likes:item.likes,collections:item.collections,cate:item.cate}}">
            <div>
              <p>
                <span class="cate">{{item.cate}}</span>
                <span class='title'>{{ item.title }}</span>
              </p>

              <p class="discribe">
                <span class="ctime">{{ item.ctime}} </span>
                <span class="read_num">阅读：{{numList[index].read_num}}</span>
                <span class="likes">喜欢:{{comHot[index].likes}}</span>
                <span class="collections">收藏:{{comHot[index].collections}}</span>
              </p>
            </div>
          </router-link>
        </van-cell>
      </van-list>
    </div>

    <bottomBar></bottomBar>

  </div>
</template>

<script>
  import bottomBar from "./bottomBar.vue";
  import common from './common.vue'
  export default {
    name: 'hotLists',
    data() {
      return {
        hotContent: [],
        numList: [],
        comHot:common.hotList,
        isActive: true,
        vanListLoading: false, // 加载状态
        finished: false, // 是否加载
        finishedText: '', // 加载完成后的提示文案

        scrollIn: 0, //进入页面时滚动条位置
        scrollOut: 0, //离开页面时滚动条位置

        keepAlive:false,  //是否需要缓存

      };
    },
    components: {
      bottomBar
    },
    methods: {
      getList() {
        let url = '/recsys/hot_list?' + 'user_id=' + common.user.username 
        console.log(common.user);
        this.axios.get(url).then(res => {
          if (res.data.code === 200) {
            this.hotContent.push(...res.data.data)
            common.hotList.push(...res.data.data)
            this.numList.push(...res.data.data)
            this.vanListLoading = false
          }
        })
      },
      onLoad() {
        this.getList();
      },
      toRec() {
        this.$router.push('/recLists')
      },
      toHot() {
        this.$router.push('/hotLists')
      }
    },
    // activated() {
    //   if(this.keepAlive){
    //     // 进入该组件后读取数据变量设置滚动位置
    //     document.documentElement.scrollTop = this.scrollOut;
    //     console.log('alive');
    //   }else{
    //     console.log('not alive');
    //   }
    // },

    // beforeRouteEnter(to, from, next)  {
    //   if(from.name !== 'signIn' && from.name !== 'signUp'){
    //     next(vm => {
    //       vm.keepAlive = true
    //       console.log(vm.keepAlive);
    //     })
    //   }else{
    //     next(vm => {
    //       from.meta.keepAlive = false;
    //       vm.keepAlive = false
    //       console.log(vm.keepAlive);
    //     })
    //   }
    // },

    // beforeRouteLeave(to, from, next) {
    //   this.scrollOut = document.documentElement.scrollTop;
    //   if(to.name == 'NewsInfo' ){
    //       let reg = /NewsInfo\//
    //       for(let i = 0; i<this.numList.length; i++){
    //         if(this.numList[i].news_id == to.path.split(reg)[1]){
    //           this.numList[i].read_num++
    //         }
    //       }
    //   }
    //   next();
    // },
  }
</script>

<style scoped>
  @import url('../assets/css/test.css');

  .lists {
    position: relative;
    top: 70px;
    padding: 0 10px;
  }

  .title {
    font-size: 1.2rem;
    color: black;
    font-weight: bolder;
  }

  .cate {
    font-size: 1rem;
    color: #00F;
    margin-right: .5rem;
    padding: .08rem;
    border: 1px solid #00F
  }

  .discribe {
    display: flex;
    justify-content: space-between;
    font-size: 1rem;
  }

  .hot {
    border-bottom: 2px solid #428bff
  }

  .hotCont {
    cursor: default;
    color: #428bff;
  }

  .hotPath {
    fill: #428bff;
  }
</style>