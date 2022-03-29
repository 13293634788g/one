<template>
  <div>
	  <el-collapse v-model="activeName" accordion>
	    <el-collapse-item title="首页" name="1">
	    </el-collapse-item>
	    <el-collapse-item title="文章管理" name="2">
	      <div>文章列表</div>
	      <div>多层路由设置</div>
	    </el-collapse-item>
	  </el-collapse>
  </div>
</template>

<script>


export default {
	name: "Home",
	data() {
	      return {
	        activeName: '1'
	      };
	    }
};
</script>
