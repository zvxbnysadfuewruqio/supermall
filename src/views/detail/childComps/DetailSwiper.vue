<template>
  <div>
    <swiper class="detailSwiper">
      <swiper-item v-for="item in banners" :key="item">
          <img :src="item">
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
  import {Swiper,SwiperItem} from 'components/common/swiper'
  export default {
    name:'',
    props:{
      banners:{
        type:Array,
        default(){
          return []
        }
      }
    },
    data(){
      return{
        isLoad:false
      }
    },
    methods:{

    },
    components:{
      Swiper,
      SwiperItem
    }
  }
</script>

<style scoped>
.detailSwiper{
  height:300px;
  overflow: hidden;
}
</style>