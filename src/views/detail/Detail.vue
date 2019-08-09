<template>
  <div>
    <detail-nav-bar></detail-nav-bar>
    <detail-swiper :banners="topImages"></detail-swiper>
  </div>
</template>

<script>
  import DetailNavBar from './childComps/DetailNavBar'
  import DetailSwiper from './childComps/DetailSwiper'

  import {getdetail,Goods} from 'network/detail'

  export default {
    name:'Detail',
    components:{
      DetailNavBar,
      DetailSwiper
    },
    data(){
      return{
        iid:0,
        topImages:[],
        goods:null,
      }
    },
    created(){
      //保存iid
      this.iid=this.$route.query.iid

      //请求数据
      getdetail(this.iid).then(res=>{
        //获取轮播图图片
        const data=res.result
        this.topImages=data.itemInfo.topImages

        //获取商品信息
        this.goods=new Goods(data.itemInfo,data.columns,data.shopInfo.services)
      })
    }
  }
</script>

<style scoped>

</style>