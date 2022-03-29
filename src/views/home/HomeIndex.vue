<template>
  <div>
    <div class="top">
        <el-row>
            <el-button type="primary">编辑添加,字段各种规则验证示例</el-button>
            <el-button type="primary">本地导出表格</el-button>
        </el-row>
         <el-select v-model="value" placeholder="请选择">
    <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value">
    </el-option>
  </el-select>
    </div>
    <el-table :data="arrs">
      <el-table-column prop="name" label="文章标题"> </el-table-column>
      <el-table-column prop="text" label="作者"> </el-table-column>
      <el-table-column prop="look" label="浏览量"> </el-table-column>
      <el-table-column prop="end" label="回复"> </el-table-column>
      <el-table-column label="操作">
        <!-- 编辑删除 -->
        <template slot-scope="scope" style="display: flex">
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)"
            >删除</el-button
          >
        </template>
        <!--  -->
      </el-table-column>
    </el-table>
    <!-- 弹窗 -->
    <el-dialog
      :title="i === 1 ? '添加用户' : '编辑'"
      :visible.sync="dialogFormVisible"
    >
      <el-form :model="form">
        <el-form-item label="文章标题" :label-width="formLabelWidth">
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="作者" :label-width="formLabelWidth">
          <el-input v-model="form.text" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="浏览量" :label-width="formLabelWidth">
          <el-input v-model="form.look" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="回复" :label-width="formLabelWidth">
          <el-input v-model="form.end" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="fnsure">确 定</el-button>
      </div>
    </el-dialog>
  </div>
  <!-- 弹出 -->
</template>
<script>
import instance from "../../api/index";
export default {
  name: "HomeIndex",
  data() {
    return {
      arrs: null,
      debounceedit: null, // 新业务防抖封装
      debouncedelete: null,
      dialogFormVisible: false, //弹窗的显示和不显示
      form: {
        id: "0",
        name: "张三",
        text: "塞军",
      },
      formLabelWidth: "100px",
      i: 0, // 区分是不是添加业务还是修改业务
    };
  },
  methods: {
    addfn(i) {
      // 添加业务
      this.i = i;
      console.log(i);
      this.form.name = "";
      this.form.text = "";
      this.dialogFormVisible = !this.dialogFormVisible;
    },
    fnsure() {
      this.dialogFormVisible = false;
      if (this.i === 1) return this.handleAdd(); // 添加业务
      this.editHomeIndex(); // 修改业务
    },
    async handleAdd() {
      // 添加业务
      let name = this.form.name;
      let text = this.form.text;
      console.log(name, text);
      let { data } = await instance.post("/api/homeindex", { name, text });
      console.log(data);
      this.getHomeIndex();
    },
    handleEdit(a, b) {
      // 用户行为
      console.log(a, b);
      this.i = 0;
      this.dialogFormVisible = !this.dialogFormVisible;
      this.form.name = b.name;
      this.form.text = b.text;
      this.form.id = b.id;
    },
    handleDelete(a, b) {
      // 用户行为
      console.log(a, b); //
      this.$confirm("您确定删除该条信息吗?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          // 确定的回调函数
          console.log("删除了");
          this.deleteHomeIndex(a);
        })
        .catch(() => {
          // 取消的回调函数
          console.log("你取消了");
        });
    },
    async getHomeIndex() {
      let { data } = await instance.get("/api/homeindex");
      console.log(data);
      this.arrs = data;
    },
    // 删除业务
    async deleteHomeIndex(index) {
      let { data } = await instance.delete("/api/homeindex/" + index);
      console.log(data);
      this.getHomeIndex();
    },
    // 编辑业务
    async editHomeIndex() {
      let name = this.form.name;
      let text = this.form.text;
      let id = this.form.id;
      console.log(name, text);
      let { data } = await instance.put("/api/homeindex/edit/" + id, {
        name,
        text,
      });
      console.log(data);
      this.getHomeIndex();
    },
  },
  created() {
    this.getHomeIndex();
  },
};
</script>
<style scoped>
.top{
  margin: 10px 0;
  display: flex;
  justify-content: flex-start;
}
.el-row{
  width: 50%;
}
.el-row button{
  width: 200px;
  font-size: 12px;
}
.el-row button span{
  text-align: center;
  color: #fff;
}
.el-button {
  width: 50px;
}
.addbtn {
  float: right;
  margin-right: 140px;
  margin-top: 40px;
  width: 100px;
}
</style>