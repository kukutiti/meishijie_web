<template>
  <div class="waterfall" ref="waterfall">
    <slot></slot>
    <div class="waterfall-loading" ref='loading' v-show="isLoading">
      <i class="el-icon-loading"></i>
    </div>
  </div>
</template>
<script>
// 什么时候到可视区中了
// waterfall 元素的下边距 < 可视区的高度 到达可视区
import {throttle} from 'throttle-debounce' //节流函数，用于设置滚动的触发间隔
export default {
  name: 'Waterfall',
  data(){
    return {
      isLoading: false
    }
  },
  mounted(){
    // 优化，每隔一段时间再去执行函数，不用频繁触发  节流函数---指定300毫秒触发一次
    // this.scrollHandler = this.scroll.bind(this); //绑定this指向
    this.scrollHandler = throttle(300, this.scroll.bind(this));
    window.addEventListener('scroll', this.scrollHandler); //绑定一个滚动事件，这里是分开写的，可以合并写
  },
  /* 万一元素不要了，先解绑再销毁 */
  destroyed(){
    window.removeEventListener('scroll', this.scrollHandler)
  },
  methods:{
    scroll(){
      console.log(123)
      if(this.isLoading) return; //如果正在刷新，就不再次刷新了 
      if(this.$refs.waterfall.getBoundingClientRect().bottom < document.documentElement.clientHeight){
        console.log('已到达可视区')
        this.isLoading = true;//到达可视区，再加载页面
        this.$emit('view')//触发外面的事件，说这里加载好了,在Home中监听
      }
    }
  }
}
</script>
<style lang="stylus">
.waterfall-loading
  width 100%
  height 20px
  text-align center
</style>