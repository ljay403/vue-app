<template>
  <div class="weather-station">
    <div class="crumbs">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item>
          <i class="el-icon-tickets"></i> 气象站
        </el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div v-if="weatherStationFlag" class="content">
      <el-row :gutter="20" v-for="(x, i) in weatherStationState" :key="i" class="row">
        <el-col :span="24">
          <x-weather-station :info="x"></x-weather-station>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import XWeatherStation from "./WeatherStationComponent.vue";

export default {
  components: {
    XWeatherStation
  },
  data() {
    return {
      // 气象站是否准备就绪
      weatherStationFlag: false,
      // 用户所有的气象站参数(实时数据)
      weatherStationState: [],
      // 定时器的名称
      update: null
    };
  },
  methods: {
    // 获得气象站实时数据
    getWeatherStationState() {
      this.axios({
        url: "http://localhost:3000/api/deviceInfo",
        params: {
          token: localStorage.getItem("user_token"),
          type: "device_weather"
        }
      })
        .then(res => {
          this.weatherStationState = [];
          for (let i in res.data) {
            this.weatherStationState.push(res.data[i]);
          }
          this.weatherStationFlag = true;
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  activated() {
    this.getWeatherStationState();
    // 每10秒更新一次数据
    this.update = setInterval(() => {
      // console.log("更新数据！");
      this.getWeatherStationState();
    }, 10000);
  },
  deactivated() {
    clearInterval(this.update);
  }
};
</script>

<style scoped>
.row {
  margin-bottom: 10px;
}
</style>