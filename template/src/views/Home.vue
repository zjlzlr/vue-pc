<template>
  <el-row class="container">
    <el-col :span="24" class="header">
      <el-col :span="10" class="logo" :class="collapsed?'logo-collapse-width':'logo-width'">
        <span v-text="collapsed?'':sysName"></span>
      </el-col>
      <el-col :span="10">
        <div class="tools" @click.prevent="collapse">
          <i class="el-icon-menu"></i>
        </div>
      </el-col>
      <el-col :span="4" class="userinfo">
        zlr
      </el-col>
    </el-col>
    <el-col :span="24" class="main">
      <aside :class="collapsed?'menu-collapsed':'menu-expanded'">
        <!--导航菜单-->
        <el-menu :default-active="$route.path" :key="collapsed" class="el-menu-vertical-demo" @open="handleopen" @close="handleclose" @select="handleselect"
           unique-opened router v-show="!collapsed">
          <template v-for="(item,index) in $router.options.routes" v-if="!item.hidden">
            <el-submenu :index="index+''" v-if="!item.leaf" v-bind:key="index">
              <template slot="title">
                <i :class="item.iconCls"></i>
                <span v-text="item.name"></span>
              </template>
              <el-menu-item v-for="child in item.children" :index="child.path" :key="child.path" v-if="!child.hidden">
                <span v-text="child.name"></span>
              </el-menu-item>
            </el-submenu>
            <el-menu-item v-if="item.leaf&&item.children.length>0" :index="item.children[0].path" v-bind:key="index">
              <i :class="item.iconCls"></i>
              <span v-text="item.children[0].name"></span>
            </el-menu-item>
          </template>
        </el-menu>
        <!--导航菜单-折叠后-->
        <ul class="el-menu el-menu-vertical-demo collapsed" v-show="collapsed" :key="collapsed" ref="menuCollapsed">
          <li v-for="(item,index) in $router.options.routes" v-if="!item.hidden" class="el-submenu item" v-bind:key="index">
            <template v-if="!item.leaf">
              <div class="el-submenu__title" style="padding-left: 20px;" @mouseover="showMenu(index,true)" @mouseout="showMenu(index,false)"><i :class="item.iconCls"></i></div>
              <ul class="el-menu submenu" :class="'submenu-hook-'+index" @mouseover="showMenu(index,true)" @mouseout="showMenu(index,false)"> 
                <li v-for="child in item.children" v-if="!child.hidden" :key="child.path" class="el-menu-item" style="padding-left: 40px;" :class="$route.path==child.path?'is-active':''" @click="$router.push(child.path)">
                  <span v-text="child.name"></span>
                </li>
              </ul>
            </template>
            <template v-else>
              <li class="el-submenu">
                <div class="el-submenu__title" style="padding-left: 20px;height: 56px;line-height: 56px;padding: 0 20px;" :class="$route.path==item.children[0].path?'is-active':''" @click="$router.push(item.children[0].path)"><i :class="item.iconCls"></i></div>
              </li>
            </template>
          </li>
        </ul>
      </aside>
      <section class="content-container" v-loading="loading">
        <div class="grid-content bg-purple-light">
          <el-col :span="24" class="breadcrumb-container">
            <strong class="title" v-text="$route.name"></strong>
            <el-breadcrumb separator="/" class="breadcrumb-inner">
              <el-breadcrumb-item v-for="item in $route.matched" :key="item.path">
                <span v-text="item.name"></span>
              </el-breadcrumb-item>
            </el-breadcrumb>
          </el-col>
          <el-col :span="24" class="content-wrapper">
            <transition name="fade" mode="out-in">
              <router-view></router-view>
            </transition>
          </el-col>
        </div>
      </section>
    </el-col>
  </el-row>
</template>

<script>
export default {
  data () {
    return {
      sysName: '正和岛',
      collapsed: false,
      sysUserName: '',
      sysUserAvatar: '',
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      }
    }
  },
  computed: {
    loading () {
      return this.$store.state.loading
    }
  },
  methods: {
    onSubmit () {
      console.log('submit!')
    },
    handleopen () {
      // console.log('handleopen')
    },
    handleclose () {
      // console.log('handleclose')
    },
    handleselect (a, b) { },
    // 退出登录
    logout () {
      this.$confirm('确认退出吗?', '提示', {
        // type: 'warning'
      })
        .then(() => {
        })
        .catch(() => { })
    },
    // 折叠导航栏
    collapse () {
      this.collapsed = !this.collapsed
    },
    showMenu (i, status) {
      this.$refs.menuCollapsed.getElementsByClassName(
        'submenu-hook-' + i
      )[0].style.display = status ? 'block' : 'none'
    }
  },
  mounted () {
  }
}
</script>

<style scoped lang="less">
@import "../style/color";
.container {
  position: absolute;
  top: 0px;
  bottom: 0px;
  width: 100%;
  .header {
    height: 60px;
    line-height: 60px;
    background: @color-blue2;
    color: #fff;
    .userinfo {
      text-align: right;
      padding-right: 35px;
      float: right;
      .userinfo-inner {
        cursor: pointer;
        color: #fff;
        img {
          width: 40px;
          height: 40px;
          border-radius: 20px;
          margin: 10px 0px 10px 10px;
          float: right;
        }
      }
    }
    .logo {
      //width:180px;
      height: 60px;
      font-size: 22px;
      padding-left: 20px;
      padding-right: 20px;
      border-color: rgba(238, 241, 146, 0.3);
      border-right-width: 1px;
      border-right-style: solid;
      img {
        width: 40px;
        float: left;
        margin: 10px 10px 10px 18px;
      }
      .txt {
        color: #fff;
      }
    }
    .logo-width {
      width: 180px;
      background-color: @color-blue1;
    }
    .logo-collapse-width {
      width: 60px;
      background-color: @color-blue1;
    }
    .tools {
      padding: 0px 23px;
      width: 14px;
      height: 60px;
      line-height: 60px;
      cursor: pointer;
    }
  }
  .main {
    display: flex;
    position: absolute;
    top: 60px;
    bottom: 0px;
    overflow: hidden;
    aside {
      flex: 0 0 180px;
      width: 180px;
      .el-menu {
        height: 100%;
      }
      .el-submenu .el-menu-item{
        min-width: 180px;
      }
      .collapsed {
        width: 60px;
        .item {
          position: relative;
        }
        .submenu {
          position: absolute;
          top: 0px;
          left: 60px;
          z-index: 99999;
          height: auto;
          display: none;
        }
      }
    }
    .menu-collapsed {
      flex: 0 0 60px;
      width: 60px;
    }
    .menu-expanded {
      flex: 0 0 180px;
      width: 180px;
    }
    .content-container {
      flex: 1;
      overflow-y: scroll;
      padding: 20px;
      .breadcrumb-container {
        .title {
          width: 180px;
          float: left;
          color: #475669;
        }
        .breadcrumb-inner {
          float: right;
        }
      }
      .content-wrapper {
        background-color: #fff;
        box-sizing: border-box;
      }
    }
  }
}
</style>
