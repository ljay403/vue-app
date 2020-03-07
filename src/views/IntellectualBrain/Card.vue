<template>
  <el-card shadow="hover">
    <div slot="header">
      <span>{{title}}</span>
    </div>
  </el-card>
</template>

<script>
export default {
  props: {
    title: {
      type: String
    }
  }
};
</script>

<style>
</style>