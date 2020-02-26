<template>
  <el-container class="home-container">
    <!--头部-->
    <el-header>
      <div>
        <img src="../assets/heima.png" alt="">
        <span>电商管理后台系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!--主体-->
    <el-container>
      <!--左侧边-->
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle-button"  @click="toggleCollapse">|||</div>
        <el-menu :router="true" background-color="#333744" :collapse="isCollapse" unique-opened text-color="#fff" active-text-color="#409eff" :collapse-transition="false">
          <!--一级菜单-->
          <el-submenu :index="item.id + ''" v-for="item in menulist" :key="item.id">
            <template slot="title">
              <!--图片-->
              <i :class="iconlist[item.id]"></i>
              <!--文本-->
              <span>{{item.authName}}</span>
            </template>
            <!--二级菜单-->
            <el-menu-item :index="'/' + subitem.path" v-for="subitem in item.children" :key="subitem.id">
              <i class="el-icon-menu"></i>
              <span>{{subitem.authName}}</span>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!--右侧-->
      <el-main>
        <!--路由占位符-->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data () {
    return {
      menulist: [],
      iconlist: {
        '125': 'iconfont icon-user',
        '103': 'iconfont icon-tijikongjian',
        '101': 'iconfont icon-shangpin',
        '102': 'iconfont icon-danju',
        '145': 'iconfont icon-baobiao'
      },
      isCollapse: false
    }
  },
  created () {
    this.getMenuList()
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('login')
    },
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      console.log(res)
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
    },
    // 点击按钮切换菜单折叠 与展开
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    }
  }
}
</script>

<style lang="less" scoped>
  .home-container{
    height: 100%;
  }
  .el-header{
    background-color: #373d41;
    display: flex;
    justify-content: space-between;
    align-items: center;
    line-height: 60px;
    padding-left: 0px;
    color: #fff;
    font-size: 20px;
    > div{
      display: flex;
      align-items: center;
      span{
        margin-left: 15px;
      }
    }
  }
  .el-aside{
    background-color: #333744;
    .el-menu{
      border-right: none;
    }
  }
  .iconfont{
    margin-right: 10px;
  }
  .toggle-button{
    background-color: #4A5064;
    font-size: 10px;
    line-height:24px;
    color: #fff;
    text-align: center;
    letter-spacing: 0.2em;
    cursor: pointer;
  }
</style>
