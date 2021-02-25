<template>
  <div class="home">
    <!-- 轮播 start -->
    <!-- interval：自动切换时间间隔，type类型，height高度 -->
    <el-carousel :interval="5000" type="card" height="300px">
      <!-- 遍历图片 -->
      <el-carousel-item v-for="item in banners" :key="item._id">
        <!--/detail?menuId=5d83bfba2f7cb93a4009cf98-->
        <router-link :to="{name: 'detail', query: {menuId: item._id}}">
          <img 
            :src="item.product_pic_url" 
            width="100%"
            alt=""
          >
        </router-link>
      </el-carousel-item>
    </el-carousel>
    <!-- 轮播 end -->
    <!-- 内容精选 瀑布流形式 start -->
    <div>
      <h2>内容精选</h2>
      <!-- :info='info' -->
      <waterfall ref="waterfall" @view="loadingMenuHandle"><!-- 组件中用组件需要用插槽标签 -->
        <menu-card :margin-left="13" :info="menuList"></menu-card>
      </waterfall>
    </div>
    <!-- 内容精选 瀑布流形式 end -->
  </div>
</template>

<script>
import MenuCard from '@/components/menu-card.vue'
import Waterfall from '@/components/waterfall.vue'
import {getBanner, getMenus} from '@/service/api.js'
// 引入 注册 使用
export default {
  name: 'home',
  components: {
    MenuCard: MenuCard,
    Waterfall
  },
  //声明一下标签要用的数据
  data(){
    return {
      banners: [],
      menuList: [],
      page: 1
    }
  },
  mounted(){
    // let data = await banner(); //也行
    getBanner().then((data) => {
      this.banners = data.data.list;
      // console.log(data+"1")
    })
    getMenus({page: this.page}).then((data) => {
      this.menuList = data.data.list;
      this.pages = Math.ceil(data.data.total/data.data.page_size);
    })
  },
  methods:{
    loadingMenuHandle(){
      console.log('在外监听的，已到可视区')
      this.page++; //滚动条后，加载更多信息，page++
      if(this.page > this.pages){
        this.$refs.waterfall.isLoading = false; //this.$refs：拿到组件元素，再拿到组建中对应的ref=waterfall的元素
        return;
      }
      this.$refs.waterfall.isLoading = true;

      getMenus({page: this.page}).then((data) => {
        this.menuList.push(...data.data.list);
        // console.log(this.menuList)
        this.$refs.waterfall.isLoading = false;
      })

    }
  }
}
</script>
<style lang="stylus">
.home 
  h2
    text-align center
    padding 20px 0;

  .el-carousel__item h3 
    color #475669
    font-size 14px
    opacity 0.75
    line-height 200px
    margin 0
  

  .el-carousel__item:nth-child(2n) 
    background-color #99a9bf
  

  .el-carousel__item:nth-child(2n+1) 
    background-color #d3dce6
  
</style>

