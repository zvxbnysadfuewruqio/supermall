<template>
  <div>
    <swiper>
      <swiper-item v-for="item in banners" :key="item.acm">
        <a :href="item.link">
          <img :src="item.image" @load="itemImage">
        </a>
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
      itemImage(){
        if(!this.isLoad){
          this.$emit('itemswiperImage')
          this.isLoad=true
        }
      }
    },
    components:{
      Swiper,
      SwiperItem
    }
  }
</script>

<style scoped>
</style>