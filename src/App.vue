<template>
  <div id="app">
    <el-container>
      <Header></Header>
      <el-main>
        <div class="main">
          <!--路径对应的组件会替换在这个位置-->
          <router-view></router-view>
        </div>
      </el-main>
      <el-footer>
        <div class="footer">Copyright © 2019 - 2019</div>
      </el-footer>
    </el-container>
  </div>
</template>
<script>
import Header from "@/components/header";
import { getBanner } from "@/service/api.js";
export default {
  components: {
    Header,
  },
  async created() {
    //请求返回数据是异步的
    // getBanner.then((data)=>console.log(data));  这种方式获取也行
    const data = await getBanner();
    console.log(data);
  },
};
</script>

<style lang="stylus">
h3, h2, h5, h1, h4, li {
  margin: 0;
}

ul {
  padding: 0;
}

a {
  text-decoration: none;
  font-size: 12px;
  color: #000;
}

ul {
  list-style: none;
}

h3, h2, h5, h1 {
  font-weight: normal;
}

em, strong {
  font-style: normal;
  font-weight: normal;
}

.clearfix:after {
  content: '';
  display: block;
  height: 0;
  clear: both;
}

#app {
  .eaeaea {
    background: #eaeaea;
    color: #666;
    border: none;
  }

  .eaeaea:hover {
    background: #ff3232;
    color: #fff;
  }
}

body {
  background: #fae8c8;
  font-size: 12px;
}

.main {
  width: 990px;
  margin: 0 auto;
}

.footer {
  text-align: center;
  height: 42px;
  line-height: 42px;
}
</style>
