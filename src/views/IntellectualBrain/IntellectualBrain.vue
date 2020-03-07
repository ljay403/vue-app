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
        <card title="气象站" />
        <card title="温室环境参数" />
      </el-col>
      <el-col :span="12">
        <card title="土壤指标" />
        <card title="农作物生长参数" />
      </el-col>
    </el-row>
  </div>
</template>

<script>
import Card from "./Card";

export default {
  components: {
    Card
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