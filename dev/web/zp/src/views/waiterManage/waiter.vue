/*
 * @Author: subx 
 * @Date: 2019-12-28 16:35:32 
 * @Last Modified by: subx
 * @Last Modified time: 2019-12-28 17:22:25
 */

<template>
  <div id="waiterList">
    <div class="searchDiv">
      <el-select size="mini" v-model="status" clearable placeholder="状态">
        <el-option
          v-for="item in statusData"
          :key="item.id"
          :label="item"
          :value="item"
        ></el-option>
      </el-select>
      <el-select size="mini" v-model="gender" clearable placeholder="性别">
        <el-option v-for="item in genderData" :key="item.id" :label="item" :value="item"></el-option>
      </el-select>
      
    </div>
    
    <div class="tableDiv">
      <el-table
        ref="multipleTable"
        :data="waiterData"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="selectionChange"
      >
        <!-- <el-table-column align="center" type="selection" width="55"></el-table-column> -->
        <el-table-column align="center" prop="id" label="id"></el-table-column>
        <el-table-column align="center" prop="username" label="用户名"></el-table-column>
        <el-table-column align="center" prop="realname" label="姓名"></el-table-column>
        <el-table-column align="center" prop="gender" label="性别"></el-table-column>
        <el-table-column align="center" prop="status" label="状态">
          <!-- <template slot-scope="scope">{{scope.row.province}}-{{scope.row.city}}</template> -->
        </el-table-column>
        
        <el-table-column align="center" label="分配工作">
          <template slot-scope="scope">
            <el-button @click="toFen(scope.row)" type="text" size="small">分配</el-button>
          </template>
        </el-table-column>
        <el-table-column align="center" label="操作" width="100">
          <template slot-scope="scope">
            
            <el-button type="text" size="small" @click="toDelete(scope.row.id)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
import {
  
  findAllCustomerService,
  findCustomerServiceByEducation,
  findCustomerServiceById,
  findCustomerServiceByGender,
  findCustomerServiceByUsername,
  saveOrUpdateCustomerService
} from "@/api/customer.js";
import config from "@/utils/config.js";
export default {
  data() {
    return {
      waiter: "",
      id:"",
      realname:"",
      gender:"",
      status:"",
      waiterData:[],
      idData:[],
      realnameData:[],
      genderData:[],
      statusData:[],
      
    };
  },
  computed: {
    waiterList() {
      let temp = [...this.waiterData];
    }
  },
  methods: {
    toFen(row) {
      alert("分配");
    },
    //编辑
    
    //删除
    toDelete(id) {
      alert("删除");
    },
    //复选框选中切换
    selectionChange(val) {
      //val 就是选中的对象组成的数组
      console.log(val);
    },
    //查找客服所有信息
    async findAllWai() {
      try {
        let res = await findAllCustomerService();
        this.waiterData = res.data;
        let genderArr = res.data.map(item => {
          return item.gender;
        });
         this.genderData = [...new Set(genderArr)];
      
        
        
        let statusArr = res.data.map(item => {
          return item.status;
        });
         this.statusData = [...new Set(statusArr)];
      } catch (error) {
        config.errorMsg(this, "查找错误");
      }
    },
  },
  created() {
    this.findAllWai();
  },
  mounted() {}
};
</script>
<style scoped>
.tableDiv {
  margin-top: 10px;
}
</style>