<template>
<gao-el-container>

<font style="font-size:13px;font-family:'Microsoft YaHei'">统计时间从:</font>&nbsp;  
<el-date-picker
      
        type="date"
        placeholder="选择日期" size="mini" style="width: 130px;">
      </el-date-picker>
     
    <el-time-picker 
      placeholder="任意时间点" size="mini" style="width: 130px;">
    </el-time-picker>

&nbsp;
<font style="font-size:13px;font-family:'Microsoft YaHei'">到:</font>&nbsp;
<el-date-picker
        
        type="date"
        placeholder="选择日期" size="mini" style="width: 130px;">
      </el-date-picker>
     
    <el-time-picker 
      placeholder="任意时间点" size="mini" style="width: 130px;">
    </el-time-picker>  
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        
    <el-button type="primary" icon="el-icon-search" size="mini" style="margin-left: 3px" @click="">查询</el-button>


<br><br>
<font style="font-size:15px;font-family:'Microsoft YaHei'">
执行科室工作量统计</font>&nbsp;

 <el-table
    :data="tableData"
    border
    style="width: 100%">
    <el-table-column
      prop="date"
      label="科室名称"
      width="100" align="center">
    </el-table-column>
    <el-table-column
      prop="name"
      label="看诊人数"
      width="100" align="center">
    </el-table-column>
    <el-table-column
      prop="province"
      label="发票数量"
      width="100" align="center">
    </el-table-column>
    <el-table-column
      prop="city"
      label="西药费"
      width="100" align="center">
    </el-table-column>
    <el-table-column
      prop="address"
      label="中成药"
      width="100" align="center">
    </el-table-column>
    <el-table-column
      prop="zip"
      label="中草药"
      width="100" align="center">
    </el-table-column>
    <el-table-column
      prop="zip"
      label="挂号费"
      width="100" align="center">
    </el-table-column><el-table-column
      prop="zip"
      label="诊查费"
      width="100" align="center">
    </el-table-column><el-table-column
      prop="zip"
      label="检查费"
      width="100" align="center">
    </el-table-column><el-table-column
      prop="zip"
      label="检验费"
      width="100" align="center">
    </el-table-column><el-table-column
      prop="zip"
      label="治疗费"
      align="center">
    </el-table-column>
  </el-table>

</gao-el-container>
</template>

<script>
export default {
    methods: {
      handleClick(row) {
        console.log(row);
      }
    },

    data() {
      return {
        tableData: [{
          date: '化验科',
          name: '20',
          province: '20',
          city: '100',
          address: '200',
          zip: 300
        }]
      }
    }
  }
</script>

<style>
  
</style>