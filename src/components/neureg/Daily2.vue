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
  <table border="0" cellspacing="1" bgcolor="#990033">
  <tr valign="top">
       <td width="200" bgcolor="#fff">
           <el-table
            :data="tableData"
            border
            style="width: 100%">
            <el-table-column
              prop="aa"
              label="收款员"
              width="100" align="center">
            </el-table-column>
            <el-table-column
              prop="bb"
              label="日结日期"
              width="100" align="center">
            </el-table-column>
           
          </el-table>
       </td>
       
       <td width="750" bgcolor="#fff">

         <el-tabs type="border-card">
            <el-tab-pane label="日结单">
               
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

                  <v-searchinput1></v-searchinput1>
        </el-tab-pane>
           
        <el-tab-pane label="发票信息">
          
           <el-table
            :data="tableData2"
            border
            style="width: 100%">
            <el-table-column
              prop="aa"
              label="单据号"
              width="150" align="center">
            </el-table-column>
            <el-table-column
              prop="bb"
              label="病例号"
              width="100" align="center">
            </el-table-column>

            <el-table-column
              prop="cc"
              label="姓名"
              width="100" align="center">
            </el-table-column>

            <el-table-column
              prop="dd"
              label="发票总额"
              width="100" align="center">
            </el-table-column>

            <el-table-column
              prop="ee"
              label="结算类别"
              width="100" align="center">
            </el-table-column>

            <el-table-column
              prop="ff"
              label="状态"
              width="100" align="center">
            </el-table-column>

            <el-table-column
              prop="gg"
              label="自费支付"
              width="100" align="center">
            </el-table-column>

            <el-table-column
              prop="hh"
              label="账户支付"
              width="100" align="center">
            </el-table-column>
          </el-table>
        <v-searchinput8></v-searchinput8>
        </el-tab-pane>
      </el-tabs>
</td>

</tr>
</table>
    
  </gao-el-container>  
</template>

<script>
export default {
  data() {
    return {
      radio01:1,
      eltab01:'first',
      eltab02:'first',
      tableData: [{
          aa: '张三',
          bb: '2018-12-12',
          cc: '男'},{
          aa: '李四',
          bb: '2018-12-13',
          cc: '女'},
      ],
       tableData2: [{
          aa: '20190100001',
          bb: 'mt90001',
          cc: '张三',
          dd: '67.9',
          ee: '自费',
          ff: '有效',
          gg: '67.9',
          hh: '0' 
          },{
           aa: '20190100002',
          bb: 'mt90002',
          cc: '李四',
          dd: '14.5',
          ee: '医保',
          ff: '重打',
          gg: '14.5',
          hh: '0'},
          ],
    }
  }
}
</script>

<style>
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