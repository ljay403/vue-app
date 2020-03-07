<template>
  <el-card shadow="hover">
    <div slot="header">
      <span>{{title}}</span>
      <el-button style="float: right; padding: 3px 0" type="text" @click="changeConfig">添加</el-button>
    </div>
    <el-table :data="config" :header-cell-style="{background:'#f5f7fa'}" border>
      <el-table-column prop="state" label="预警参数" align="center"></el-table-column>
      <el-table-column prop="value1" label="最小值" align="center"></el-table-column>
      <el-table-column prop="value2" label="最大值" align="center"></el-table-column>
      <el-table-column label="操作" width="180" align="center">
        <template slot-scope="scope">
          <el-button
            type="text"
            icon="el-icon-edit"
            @click="changeConfig(scope.$index, scope.row)"
          >编辑</el-button>
          <el-button
            type="text"
            icon="el-icon-delete"
            class="red"
            @click="deleteConfig(scope.$index, scope.row)"
          >删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </el-card>
</template>

<script>
export default {
  data() {
    return {
      config: []
    };
  },
  props: {
    title: {
      type: String
    },
    type: {
      type: String
    }
  },
  methods: {
    getConfig() {
      this.axios({
        url: "http://localhost:3000/api/config",
        params: {
          token: localStorage.getItem("user_token"),
          type: this.type
        }
      })
        .then(res => {
          // console.log(res);
          this.config = [];
          this.config = res.data;
        })
        .catch(err => {
          console.log(err);
        });
    },
    addConfig() {
      this.axios({
        url: "http://localhost:3000/api/addConfig",
        params: {
          token: localStorage.getItem("user_token"),
          type: this.type
        }
      })
        .then(res => {
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        });
    },
    changeConfig() {
      this.axios({
        url: "http://localhost:3000/api/changeConfig",
        params: {
          token: localStorage.getItem("user_token"),
          type: this.type
        }
      })
        .then(res => {
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        });
    },
    deleteConfig() {
      this.axios({
        url: "http://localhost:3000/api/deleteConfig",
        params: {
          token: localStorage.getItem("user_token"),
          type: this.type
        }
      })
        .then(res => {
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  activated() {
    this.getConfig();
  }
};
</script>

<style>
.red {
  color: #ff0000;
}
</style>