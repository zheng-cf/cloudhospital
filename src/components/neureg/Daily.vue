<template>
<gao-el-container>

<font style="font-size:13px;font-family:'Microsoft YaHei'">收款员:</font>&nbsp;   
         <el-select  placeholder="收款员" style="width: 110px"
                     size="mini">
              <el-option label="张三" value="1"></el-option>
             
         </el-select><font color="red">*</font>&nbsp;&nbsp;
         &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

<font style="font-size:13px;font-family:'Microsoft YaHei'">统计日期(上次结束时间):</font>&nbsp;  
<el-date-picker
      
<el-input  placeholder="2018-12-12 18:00:00" style="width: 150px"  again="left" size="mini">
</el-input>    

&nbsp;
<font style="font-size:13px;font-family:'Microsoft YaHei'">至:</font>&nbsp;
<el-date-picker
        type="date"
        placeholder="选择日期" size="mini" style="width: 130px;">
      </el-date-picker>
     
    <el-time-picker 
      placeholder="任意时间点" size="mini" style="width: 130px;">
    </el-time-picker>  
    &nbsp;
&nbsp;

        
    <el-button type="primary" icon="el-icon-search" size="mini" style="margin-left: 3px" @click="">日结</el-button>

    <el-button type="primary" icon="el-icon-goods" size="mini" style="margin-left: 3px" @click="">保存</el-button>

</br></br>
<font style="font-size:15px;font-family:'Microsoft YaHei'">
门诊收费日结单</font>&nbsp;

<table border="0" cellspacing="1" bgcolor="#990033">
<tr bgcolor="#fff">
<td>日结时间范围</td>
<td colspan=12>{{aa}}</td>
</tr>

<tr bgcolor="#fff">
<td>制表人</td>
<td colspan=3>张三</td>
<td>收款员</td>
<td colspan=5>张三</td>
<td>制表时间</td>
<td colspan=2>2018-12-30</td>
</tr>

<tr bgcolor="#fff">
<td>起止发票号</td>
<td colspan=11>10001----83455</td>

</tr>

<tr bgcolor="#fff">
<td>作废发票号</td>
<td colspan=11>10003 70048 81890</td>

</tr>

<tr bgcolor="#fff">
<td>重打发票号</td>
<td colspan=11>12997</td>

</tr>

<tr bgcolor="#fff">
<td>收费金额</td>
<td>挂号费</td>
<td>500.00</td>
<td>药费</td>
<td>1000.00</td>
<td>其他费用</td>
<td colspan=7>30.00</td>
</tr>

<tr bgcolor="#fff">
<td>共计</td>
<td>(大写)</td>
<td colspan=4>壹仟伍佰叁拾</td>
<td>(小写)</td>
<td colspan=6>1530.00</td>
</tr>

</table>

<!--
 <el-table
      :data="tableData6"
      :span-method="arraySpanMethod"
      border style="width: 100%">

      <el-table-column
        prop="c01"
        label="列01"
        width="110">
      </el-table-column>

      <el-table-column
        prop="c02"
        label="列02"
        width="60">
      </el-table-column>

      <el-table-column
        prop="c03"
        label="列03"
        width="60">
      </el-table-column>

      <el-table-column
        prop="c04"
        label="列04"
        width="60">
      </el-table-column>

      <el-table-column
        prop="c05"
        label="列05"
        width="60">
      </el-table-column>

      <el-table-column
        prop="c06"
        label="列06"
        width="60">
      </el-table-column>

      <el-table-column
        prop="c07"
        label="列07"
        width="60">
      </el-table-column>

      <el-table-column
        prop="c08"
        label="列08"
        width="60">
      </el-table-column>

      <el-table-column
        prop="c09"
        label="列09"
        width="60">
      </el-table-column>

      <el-table-column
        prop="c10"
        label="列10"
        width="60">
      </el-table-column>

      <el-table-column
        prop="c11"
        label="列11"
        width="60">
      </el-table-column>

      <el-table-column
        prop="c12"
        label="列12"
        width="60">
      </el-table-column>

      <el-table-column
        prop="c13"
        label="列13"
        width="60">
      </el-table-column>
</el-table>
-->
</gao-el-container>
</template>

<script>
  import {postRequest}   from '../../utils/api'
  import {putRequest}    from '../../utils/api'
  import {deleteRequest} from '../../utils/api'
  import {getRequest}    from '../../utils/api'

  export default {
    data() {
      return {
        aa:'2018-12-12 18:00:00----2018-12-13 18:00:00',
        tableData6: [{
          c01: '日结时间范围',
          },
          {
          c01: '制表人',
          c02: '22',
          c03: '23',
          c04: '24',
          c05: '25',
          c06: '26',
          c07: '27',
          c08: '28',
          c09: '29',
          c10: '210',
          c11: '211',
          c12: '212',
          c13: '213'},
         ]
      };
    },
    methods: {
      arraySpanMethod({ row, column, rowIndex, columnIndex }) {
        console.log(row,column)

        if (row.c02=='12') 
        {
            if (columnIndex == 1) {
              return [1, 2];
            } 
            else if (columnIndex == 2) {
              return [0, 0];
            }

            if (columnIndex == 2) {
              return [1, 2];
            } 
            else if (columnIndex == 3) {
              return [0, 0];
            }
         
        }
      },

      objectSpanMethod({ row, column, rowIndex, columnIndex }) {
        if (columnIndex === 0) {
          if (rowIndex % 2 === 0) {
            return {
              rowspan: 4,
              colspan: 4
            };
          } else {
            return {
              rowspan: 0,
              colspan: 0
            };
          }
        }
      }
    }
  };

  
 
</script>
<style>
  .cate_mana_header {
    background-color: #ececec;
    margin-top: 20px;
    padding-left: 5px;
    display: flex;
    justify-content: flex-start;
    text-align: center;
  }

  .cate_mana_main {
    /*justify-content: flex-start;*/
    display: flex;
    flex-direction: column;
    padding-left: 5px;
    background-color: #ececec;
    margin-top: 20px;
    padding-top: 10px;
    text-align: center;
  }

  .gao-cate_mana_header {
    background-color: #ececec;
    margin-top: 20px;
    padding-left: 5px;
    display: flex;
    justify-content: flex-start;
    text-align: center;
    vertical-align: middle;
  }

  .gao-cate_mana_main {
    /*justify-content: flex-start;*/
    display: flex;
    flex-direction: column;
    padding-left: 5px;
    background-color: #ececec;
    margin-top: 20px;
    padding-top: 10px;
  }

  .gao-transfer-footer {
    margin-left: 20px;
    padding: 6px 5px;
  }

  .gao-el-header {
    background-color: #20a0ff;
    color: #333;
    text-align: left;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .gao-el-aside {
    background-color: #ECECEC;
  }

  .gao-el-main {
    background-color: #fff;
    color: #000;
    text-align: left;
  }

  .gao-el-container {
    background-color: #fff;
    color: #000;
    text-align: left;
  }
</style>
