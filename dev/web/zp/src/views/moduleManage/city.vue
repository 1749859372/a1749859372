/*
 * @Author: zhough 
 * @Date: 2019-12-28 17:10:58 
 * @Last Modified by: zhough
 * @Last Modified time: 2019-12-28 18:56:54
 */
<template>
  <div id="moduleCity">
    <el-button type="warning" style="float:right" icon="el-icon-info" size="medium">添加省份</el-button><br><br><br><br>
    <el-container>
  <el-header>
    <el-input v-model="input" placeholder="请输入省份名称" style="width:150px" size="mini" ></el-input>
    <el-button size="mini">确定</el-button>
  </el-header>
  <el-main border-radius:1px>
    <el-button  class="button-new-tag" size="small" @click="showInput">添加</el-button>
    <!-- sdjadj -->
  </el-main>
</el-container>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  computed: {},
  methods: {},
  created() {},
  mounted() {}
};
</script>
<style scoped>

  .el-header {
    background-color:black;
    color: #333;
    text-align: left;
    line-height: 60px;
  }
  .el-main {
    background-color: #E9EEF3;
    color: #333;
    text-align: left;
    line-height: 60px;
  }
  /* body > .el-container {
    margin-bottom: 30px;
  } */
  
</style>