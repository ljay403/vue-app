<template>
  <el-card shadow="hover">
    <div slot="header">
      <span>{{title}}</span>
      <el-button
        style="float: right; padding: 3px 0"
        type="text"
        @click="configFormVisible = true"
      >添加</el-button>
    </div>
    <el-table :data="config" :header-cell-style="{background:'#f5f7fa'}" border>
      <el-table-column prop="name" label="预警参数" align="center"></el-table-column>
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
    <!-- Form Start -->
    <el-dialog title="添加规则" :visible.sync="configFormVisible" width="30%">
      <el-form :model="form" label-width="auto">
        <el-form-item label="预警参数">
          <el-select v-model="form.state" placeholder="预警参数">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="`${item.label}|${item.value}`"
            ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="安全数值">
          <el-col :span="11">
            <el-input v-model="form.value1" autocomplete="off" placeholder="最小值"></el-input>
          </el-col>
          <el-col class="line" :span="2">-</el-col>
          <el-col :span="11">
            <el-input v-model="form.value2" autocomplete="off" placeholder="最大值  "></el-input>
          </el-col>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="configFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="addConfig(form)">确 定</el-button>
      </div>
    </el-dialog>
    <!-- Form End -->
  </el-card>
</template>

<script>
export default {
  data() {
    return {
      config: [],
      configFormVisible: false,
      form: {
        name: "",
        state: "",
        value1: "",
        value2: ""
      }
    };
  },
  props: {
    title: {
      type: String
    },
    type: {
      type: String
    },
    options: {
      type: Array
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
    addConfig(form) {
      let [a, b] = form.state.split("|"); // es6 数组解构赋值
      this.axios({
        url: "http://localhost:3000/api/addConfig",
        params: {
          token: localStorage.getItem("user_token"),
          name: a,
          state: b,
          type: this.type,
          value1: form.value1,
          value2: form.value2
        }
      })
        .then(res => {
          // console.log(res);
          if (res.data.appcode === "1") {
            this.$router.go(0);
          }
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
    deleteConfig(index, content) {
      this.axios({
        url: "http://localhost:3000/api/deleteConfig",
        params: {
          token: localStorage.getItem("user_token"),
          type: this.type,
          state: content.state
        }
      })
        .then(res => {
          // console.log(res);
          if (res.data.appcode === "1") {
            this.$router.go(0);
          }
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