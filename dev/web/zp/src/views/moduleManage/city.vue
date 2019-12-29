/*
 * @Author: zhough 
 * @Date: 2019-12-28 17:10:58 
 * @Last Modified by: zhough
 * @Last Modified time: 2019-12-29 10:40:06
 */
<template>
  <div id="moduleCity">
    <!-- {{moduleCity}} -->
    <!-- {{provinceData}}------- -->
    <!-- {{province}} 
    {{city}}-->
    <!-- {{cityData}}  -->
    <el-table
        ref="multipleTable"
        :data="moduleCity"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="selectionChange"
      >
      </el-table>
    <el-button type="warning" style="float:right" icon="el-icon-info" size="medium">添加省份</el-button><br><br><br><br>
    <el-container>
  <el-header>
    <el-select @change="dialogProChange" size="mini" v-model="province" clearable placeholder="请输入省份">
        <el-option
          v-for="item in provinceData"
          :key="item.id"
          :label="item.name"
          :value="item.id"
        ></el-option>
      </el-select>
  </el-header>
  <el-main>
    <el-table-column  label="城市">
          <template slot-scope="scope">{{scope.row.city}}</template>
        </el-table-column>
    <el-tag
  :key="tag"
  v-for="tag in cityData"
  closable
  :disable-transitions="false"
  @close="handleClose(tag)"
  >
  
  {{tag}}
</el-tag>
<el-input
  class="input-new-tag"
  v-if="inputVisible"
  v-model="inputValue"
  ref="saveTagInput"
  size="small"
  @keyup.enter.native="handleInputConfirm"
  @blur="handleInputConfirm"
>
</el-input>

<el-button v-else class="button-new-tag" size="small" @click="showInput" >添加</el-button>
  </el-main>
</el-container>
  </div>
</template>
<style>
  .el-tag + .el-tag {
    margin-left: 10px;
  }
  .button-new-tag {
    margin-left: 10px;
    height: 32px;
    line-height: 30px;
    padding-top: 0;
    padding-bottom: 0;
  }
  .input-new-tag {
    width: 90px;
    margin-left: 10px;
    vertical-align: bottom;
  }
</style>
<script>
import { findAllProvince } from "@/api/province.js";
import { findAllCity, findCityByProvinceId } from "@/api/city.js";
  
import config from "@/utils/config.js";
export default {
  data() {
    return {
      // dynamicTags: ['sss1'],
      inputVisible: false,
        inputValue: '',
        //省份列表，表格数据
       moduleCity: [],
       //省份
      province: "",
      //城市
      city: "",
      //省份数组
      provinceData: [],
      //城市数组
      cityData: [],
      //省份对应的城市信息
      provinceCityData: [],
      //
      moduleCity:[],

    };
  },
  computed: {},
  methods: {
    selectionChange(val) {
      //val 就是选中的对象组成的数组
      // console.log(val);
      let ids = val.map(item => {
        return item.id;
      });
      this.ids = ids;
    },
    //查找所有省份信息
    async findAllPro() {
      try {
        let res = await findAllProvince();
        this.provinceData = res.data;
      } catch (error) {
        config.errorMsg(this, "查找错误");
      }
    },
    //查找所有城市信息
    async findAllCi() {
      try {
        let res = await findAllCity();
        // this.cityData = res.data;
        // let temp = [...res.data];
        // let nameArr = res.data.map( item =>{
        //   return item.name;
        // })
        // this.cityData = nameArr;
      } catch (error) {
        config.errorMsg(this, "查找错误");
      }
    },
    //省份发生改变
    async provinceChange(val) {
      this.city = "";
      this.industry = "";
      this.scale = "";
      //val 是option的value值
      if (val) {
        try {
          let res = await findCityByProvinceId({ province: val });
          this.businessData = res.data;
          this.currentPage = 1;
        } catch (error) {
          config.errorMsg(this, "通过省份查找信息错误");
        }
      } else {
        this.findAllPro();
      }
    },
    async dialogProChange(val) {
      console.log(val);
      //通过省份id获取城市
      try {
        let res = await findCityByProvinceId({ provinceId: val });
        console.log('-------------', res.data);
        let nameArr = res.data.map( item =>{
          return item.name;
        })
        this.cityData = nameArr;
      } catch (error) {
        console.log(error);
        config.errorMsg(this, "通过省份查找城市失败");
      }
    },

    handleClose(tag) {
        this.cityData.splice(this.cityData.indexOf(tag), 1);
      },
  
    showInput() {
        this.inputVisible = true;
        this.$nextTick(_ => {
          this.$refs.saveTagInput.$refs.input.focus();
        });
      },

      handleInputConfirm() {
        let inputValue = this.inputValue;
        if (inputValue) {
          this.cityData.push(inputValue);
        }
        this.inputVisible = false;
        this.inputValue = '';
      },
  },
  created() {
    this.findAllPro();
    this.findAllCi();
  },
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
    line-height: 40px;
  }
</style>