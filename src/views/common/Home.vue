<template lang="pug">
  .home-container
    left-bar(:isLeftBarShow='isLeftBarShow')
    .right-content(:style='{marginLeft: isLeftBarShow ? "220px" : "0px"}')
      .topbar
        a.togglebtn(@click='isLeftBarShow = !isLeftBarShow' href='javascript:void(0)')
          img(src='../../assets/menu.png')
        el-button(type='danger' size='mini' @click='loginOut').loginout 登出
      .page-content
        .page-breads
          el-breadcrumb(separator='/')
            el-breadcrumb-item(v-for='item in $route.meta.breads') {{item}}
          //- el-button(@click='$router.go(-1)').back-btn 返回
        .router-container(v-loading='isLoading')
          router-view.router
</template>

<script>
import LeftBar from '@/components/LeftBar'

export default {
  data() {
    return {
      isLeftBarShow: true
    }
  },
  computed: {
    isLoading() {
      return this.$store.state.isLoading
    }
  },
  components: {
    LeftBar
  },
  methods: {
    loginOut() {
      localStorage.clear()
      this.$router.replace({
        name: 'login'
      })
    }
  }
}
</script>


<style lang="less"  scoped>
.right-content {
  margin-left: 220px;
  position: relative;
  padding: 0 10px 0;
  transition: .3s;
  .topbar {
    height: 40px;
    background-color: #fff;
    position: relative;
    margin-bottom: 5px;
    .loginout {
      position: absolute;
      right: 5px;
      top: 6px;
    }
    .back-btn {
      float: right;
    }
    .togglebtn {
      width: 50px;
      height: 40px;
      display: block;
      padding-top: 8px;
      padding-left: 12px;
      img {
        width: 25px;
      }
    }
    .togglebtn:hover {
      background-color: #f5f5f5;
    }
  }
  .page-content {
    background-color: #fff;
    border-radius: 5px;
    padding: 15px;
    box-shadow: 0 0 10px 3px rgba(0, 0, 0, 0.08);
  }
  .page-breads {
    border-bottom: 1px #eee solid;
    padding-bottom: 15px;
    font-weight: bold;
    text-align: left;
    font-size: 14px;
    margin-bottom: 15px;
  }
}
</style>
