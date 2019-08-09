<template>
  <div id="home" class="wrapper">
    <nav-bar class="home-nav">
      <div slot="center">首页</div>
    </nav-bar>
    <tab-control class="tab-control"
                    :titles="['流行', '新款', '精选']"
                    @tabClick="tabClick"
                    v-show="isTabFixed"
                    ref="tabControl2"/>
    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            @scroll="contentScroll"
            :pull-up-load="true"
            @pullingUp="loadMore">
      <home-swiper :banners='banners' @itemswiperImage="itemswiperImage"/>
      <recommend-view :recommends="recommends"/>
      <feature-view/>
      <tab-control :titles="['流行', '新款', '精选']"
                    @tabClick="tabClick" 
                    ref="tabControl1"/>
      
      <good-list :goods="showGoods"/>
    </scroll>
    <back-top @click.native="backClick" v-show="isShowBackTop"/>
  </div>
</template>

<script>
  import HomeSwiper from './childComps/HomeSwiper'
  import RecommendView from './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'
  import GoodList from 'components/content/goods/GoodsList'

  import NavBar from 'components/common/navbar/NavBar';
  import TabControl from 'components/content/tabControl/TabControl'
  import Scroll from 'components/common/scroll/Scroll'
  import BackTop from 'components/content/backTop/BackTop'


  import {getHomeMultidata,getHomeGoods} from "network/home"
  import {debounce} from 'common/utils.js'

  export default {
    name: "Home",
    data() {
      return {
        banners:[],//轮播图数组
        recommends:[],
        goods:{
          'pop':{page:0,list:[]},
          'new':{page:0,list:[]},
          'sell':{page:0,list:[]},
        },
        currentType:'pop',
        isShowBackTop: false,
        tabOffsetTop:0,
        isTabFixed:false,
        saveY:0
      }
    },
    components:{
      HomeSwiper,
      RecommendView,
      FeatureView,
      NavBar,
      TabControl,
      GoodList,
      Scroll,
      BackTop
    },
    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      }
    },
    created(){
      this.getHomeMultidata()
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')

    },
    mounted(){
      //图片加载完成的事件监听
      const refresh=debounce(this.$refs.scroll.refresh,500)
      this.$bus.$on('itrmImageLoad',()=>{
        refresh()
      })
    },
    methods: {
      tabClick(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
        this.$refs.tabControl1.currentIndex=index
        this.$refs.tabControl2.currentIndex=index
      },
      backClick() {
        this.$refs.scroll.scrollTo(0, 0)
      },
      contentScroll(position) {
        //判断BackTop是否显示
        this.isShowBackTop = (-position.y) > 1000

        //判断tabCon是否显示
        this.isTabFixed=(-position.y)>=this.tabOffsetTop
      },
      loadMore() {
        this.getHomeGoods(this.currentType)
      },
      itemswiperImage(){
        //获取tabControl1的OffsetTop
        this.tabOffsetTop=this.$refs.tabControl1.$el.offsetTop
      },
      getHomeMultidata(){
        getHomeMultidata().then(res=>{
          this.banners=res.data.banner.list;
          this.recommends=res.data.recommend.list
          this.recommends=res.data.recommend.list
        })
      },
      getHomeGoods(type){
        const page = this.goods[type].page+1
        getHomeGoods(type,page).then(res=>{
          this.goods[type].list.push(...res.data.list);
          this.goods[type].page+=1

          this.$refs.scroll.finishPullUp()
        })
      }
    },
    destroyed(){

    },
    activated(){
      this.$refs.scroll.refresh()
      this.$refs.scroll.scrollTo(0,this.saveY,0)
    },
    deactivated(){
      this.saveY=this.$refs.scroll.getScorllY()
    }
  }
</script>

<style scoped>
  #home {
    /*padding-top: 44px;*/
    height: 100vh;
    position: relative;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    /* position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9; */
  }

  .tab-control {
    position: relative;
    left:0;
    right: 0;
    z-index: 9;
  }

  .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
</style>
