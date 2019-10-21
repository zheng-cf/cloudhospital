<template>
  <!-- 药房发药 -->
	<el-container >
  		<el-header style="height: 100%">
        <el-container >
        	<!--   患者选择区      -->
        	<el-header >
            <el-form :inline="true" :model="searchForm01" class="demo-form-inline" size="mini" >
              <el-row style="margin-top: 10px;">
                <el-col :span="8">
                  <span style="color: red">*</span>
                  <span style="font-size:13px;font-family:'Microsoft YaHei'">病历号:</span>
                  <el-input v-model="searchForm01.CaseNumber" placeholder="输入病历号" size="mini" style="width: 200px"></el-input>
                </el-col>
                <el-col :span="8">
                <el-form-item label="时间：">
                  <el-date-picker  v-model="searchForm01.SearchDate" type="date"  placeholder="选择日期">
                  </el-date-picker>
                </el-form-item>
                </el-col>
                <el-col :span="8" style='child-align: left'  >
                <el-form-item >
                  <el-button type="primary" @click="searchTemplate()">查询</el-button>
                </el-form-item>
                </el-col>
              </el-row>
            </el-form>
        	</el-header>
        	<el-main >
              <!--<el-row>-->
                <!--<el-col :span="8">-->
                <!--病历号:-->
                  <!--<el-input type="text" size="mini" v-model="form01.text01" :span="8"></el-input>-->
                <!--</el-col>-->
                <!--<el-col :span="8">-->
                <!--患者姓名:-->
                  <!--<el-input type="text" size="mini" v-model="form01.text02" :span="8"></el-input>-->
                <!--</el-col>-->
                <!--<el-col :span="8">-->
                <!--年龄:-->
                  <!--<el-input type="text" size="mini" v-model="form01.text03" :span="8"></el-input>-->
                <!--</el-col>-->
              <!--</el-row>-->
        	</el-main>        		
        </el-container>
      </el-header>
    	<el-container direction="vertical" >
        <el-row style="background-color: #EAF1F5">
          <el-col :span="24">
            <el-button type="primary"  class="el-icon-success" @click="sendDrugs" :disabled="this.selItems.length==0">发药</el-button>
          </el-col>
        </el-row>
            <el-table :data="tableData01"    style="width: 100%" @selection-change="handleSelectionChange" size="mini">
              <el-table-column type="selection"  width="55">
              </el-table-column>
              <el-table-column prop="DrugsName" label="药品名称" >
              </el-table-column>
              <el-table-column prop="DrugsPrice" label="单价" width="80px">
              </el-table-column>
              <el-table-column prop="Amount" label="数量" width="50px">
              </el-table-column>
              <el-table-column prop="State" label="药品状态" width="100px">
              </el-table-column>
              <el-table-column prop="RealName" label="开立医生" width="100px">
              </el-table-column>
              <el-table-column prop="PrescriptionName" label="处方名称" width="160px">
              </el-table-column>
              <el-table-column prop="PrescriptionTime" label="开立时间" width="140px">
                <template slot-scope="scope">
                  {{ scope.row.PrescriptionTime|formatDateTime }}
                </template>
              </el-table-column>
            </el-table>
    	</el-container>        
    </el-container>
</template>
<style>
</style>
<script>
  import {formatDateTime} from "../../utils/utils";
  import {postRequest}   from '../../utils/api'
  import {putRequest}    from '../../utils/api'
  import {deleteRequest} from '../../utils/api'
  import {getRequest}    from '../../utils/api'
  export default {
    name: 'tabpage',
    data() {
        return {
          searchForm01:{CaseNumber:'',SearchDate:new Date()},
          tableData01:[],
          selItems:[],
        }
    },
    methods: {
      sendDrugs(){
        let _this = this;
        this.$confirm('确认发放这 ' +  this.selItems.length + ' 种药品?', '提示', {
          type: 'warning',
          confirmButtonText: '确定',
          cancelButtonText: '取消'
        }).then(()=> {
          let selItems = _this.selItems;
          let ids = '';
          for (let i = 0; i < selItems.length; i++) {
            ids += selItems[i].ID + ",";
          }
          //修改状态
          deleteRequest('/neudoc/prescriptionDetailed/drugs/' + ids).then(resp=> {
            let json = resp.data;
            _this.$message({
              type: json.status,
              message: json.msg
            });
            _this.searchTemplate();
          })
        }).catch(() => {
          //取消
          _this.loading = false;
        });
      },
      searchTemplate(){
        if(this.searchForm01.CaseNumber==null || this.searchForm01.CaseNumber==''){
          this.$message({type:'warning',message:'请输入病历号'});
          return false;
        }
        let _this = this;
        if(this.searchForm01.SearchDate==null || this.searchForm01.SearchDate==''){
          this.searchForm01.SearchDate=new Date();
        }
        this.searchForm01.SearchDate = formatDateTime(this.searchForm01.SearchDate);
        let url = "/neudoc/prescriptionDetailed/getPrescriptionByRegisterAndDate?caseNumber=" + this.searchForm01.CaseNumber + "&searchDate=" + this.searchForm01.SearchDate;
        getRequest(url).then(resp => {
          _this.loading = false;
          if (resp.status == 200) {
            this.tableData01 = resp.data.list;
          } else {
            _this.$message({type: 'error', message: '数据加载失败!'});
          }
        });
      },

      handleSelectionChange(val) {
        this.selItems = val;
      }
    }
}
</script>
