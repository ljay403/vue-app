<template>
  <el-card shadow="hover">
    <div slot="header">
      <span>异常参数预警</span>
    </div>
    <el-table
      :data="warning"
      :header-cell-style="{background:'#f5f7fa'}"
      border
      class="weather-station-table"
      height="200"
    >
      <el-table-column prop="fd_id" label="设备ID" align="center"></el-table-column>
      <el-table-column prop="area" label="区域" align="center"></el-table-column>
      <el-table-column prop="name" label="预警参数" align="center"></el-table-column>
      <el-table-column prop="value" label="数值" align="center"></el-table-column>
    </el-table>
  </el-card>
</template>

<script>
export default {
  data() {
    return {
      // 预警信息
      warning: [],
      // 定时器的名称
      update: null
    };
  },
  methods: {
    // 获得预警信息
    getWarning() {
      this.axios({
        url: "http://localhost:3000/api/warning",
        params: {
          token: localStorage.getItem("user_token")
        }
      })
        .then(res => {
          this.warning = [];
          this.warning = res.data;
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  activated() {
    this.getWarning();
    // 每10秒更新一次数据
    this.update = setInterval(() => {
      this.getWarning();
    }, 10000);
  },
  deactivated() {
    clearInterval(this.update);
  }
};
</script>

<style scoped>
</style>