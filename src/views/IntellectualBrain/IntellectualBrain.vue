<template>
  <div class="intellectual-brain">
    <div class="crumbs">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item>
          <i class="el-icon-cpu"></i> 智慧农业大脑
        </el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <el-row :gutter="20">
      <el-col :span="12">
        <card title="气象站" type="device_weather" :options="optionsWeather" />
        <card title="温室环境参数" type="device_greenhouse" :options="optionsSoil" />
      </el-col>
      <el-col :span="12">
        <card title="土壤指标" type="device_soil" :options="optionsGreenhouse" />
        <card title="农作物生长参数" type="device_crops" :options="optionsCrops" />
      </el-col>
    </el-row>
  </div>
</template>

<script>
import Card from "./Card";

export default {
  components: {
    Card
  },
  data() {
    return {
      optionsWeather: [
        {
          value: "rainfall",
          label: "降雨量"
        },
        {
          value: "illuminance",
          label: "光照度"
        },
        {
          value: "temperature",
          label: "温度"
        },
        {
          value: "windspeed",
          label: "风速"
        },
        {
          value: "humidity",
          label: "湿度"
        },
        {
          value: "pressure",
          label: "气压"
        }
      ],
      optionsSoil: [
        {
          value: "temperature",
          label: "温度"
        },
        {
          value: "humidity",
          label: "湿度"
        },
        {
          value: "ph",
          label: "PH"
        },
        {
          value: "ec",
          label: "EC"
        }
      ],
      optionsGreenhouse: [
        {
          value: "air_temperature",
          label: "空气温度"
        },
        {
          value: "air_humidity",
          label: "空气湿度"
        },
        {
          value: "soil_temperature",
          label: "土壤温度"
        },
        {
          value: "soil_humidity",
          label: "土壤湿度"
        },
        {
          value: "illuminance",
          label: "光照度"
        },
        {
          value: "co2",
          label: "CO2浓度"
        },
        {
          value: "soil_fertility",
          label: "土壤肥沃度"
        }
      ],
      optionsCrops: [
        {
          value: "nitrogen",
          label: "叶片氮含量"
        },
        {
          value: "photosynthesis",
          label: "光合作用速率"
        },
        {
          value: "stem_flow",
          label: "茎流"
        },
        {
          value: "stem_diameter",
          label: "茎粗"
        },
        {
          value: "plant_height",
          label: "株高"
        },
        {
          value: "plant_length",
          label: "株长"
        },
        {
          value: "leaf_area",
          label: "叶面积"
        }
      ]
    };
  }
};
</script>

<style scoped>
.el-row {
  margin-bottom: 20px;
}
.el-card {
  margin-bottom: 20px;
}
</style>