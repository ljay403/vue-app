<template>
  <div class="home">
    <x-header></x-header>
    <x-sidebar></x-sidebar>
    <div class="content-box" :class="{'content-collapse':!this.$store.state.collapse}">
      <keep-alive>
        <router-view></router-view>
      </keep-alive>
    </div>
  </div>
</template>

<script>
import XHeader from "./Header.vue";
import XSidebar from "./Sidebar.vue";
import qs from "qs";

export default {
  components: {
    XHeader,
    XSidebar
  },
  methods: {
    // 获得信息(坐标、名称等)
    getInfo() {
      this.axios({
        url: "http://localhost:3000/api/info",
        params: {
          token: localStorage.getItem("user_token")
        }
      })
        .then(res => {
          this.$store.commit("getUserInfo", res.data.userInfo);
          this.$store.commit("getOthersInfo", res.data.othersInfo);
          this.$store.commit("mapReady");
        })
        .catch(err => {
          console.log(err);
          this.$store.commit("mapReady");
        });
    },
    // 获得用户的key和secret
    getKey() {
      this.axios({
        url: "http://localhost:3000/api/key",
        params: {
          token: localStorage.getItem("user_token")
        }
      })
        .then(res => {
          this.getAccessToken(res.data.key, res.data.secret);
        })
        .catch(err => {
          console.log(err);
        });
    },
    // 获得监控accessToken
    getAccessToken(key, secret) {
      this.axios({
        method: "post",
        url: "https://open.ys7.com/api/lapp/token/get",
        data: qs.stringify({ appKey: key, appSecret: secret }),
        headers: { "Content-Type": "application/x-www-form-urlencoded" }
      })
        .then(res => {
          if (res.data.code == "200") {
            this.$store.commit("getAccessToken", res.data.data.accessToken);
            this.getCameraList(this.$store.state.userInfo.accessToken);
          } else {
            this.$message.error(res.data.msg);
          }
        })
        .catch(err => {
          console.log(err);
        });
    },
    // 获得摄像头列表
    getCameraList(token) {
      this.axios({
        method: "post",
        url: "https://open.ys7.com/api/lapp/camera/list",
        data: qs.stringify({ accessToken: token }),
        headers: { "Content-Type": "application/x-www-form-urlencoded" }
      })
        .then(res => {
          if (res.data.code == "200") {
            this.$store.commit("getCameraList", res.data.data);
          } else {
            alert(res.data.msg);
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  created() {
    this.getInfo();
    this.getKey();
  }
};
</script>

<style>
.content-box {
  position: absolute;
  left: 250px;
  right: 0;
  top: 70px;
  bottom: 0;
  -webkit-transition: left 0.3s ease-in-out;
  transition: left 0.3s ease-in-out;
  background: #f0f0f0;
  padding: 10px;
  overflow-y: auto;
}
.content-collapse {
  left: 65px;
}
.crumbs {
  margin: 10px 0;
}
.container {
  padding: 30px;
  background: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>