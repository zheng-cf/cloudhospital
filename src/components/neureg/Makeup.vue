<template>

<div class="container">

<font style="font-size:15px;font-family:'Microsoft YaHei'">
        患者信息查询</font>&nbsp;
<el-row :gutter="2">
  <el-col :span="4">
  <div class="grid-content bg-purple">
       <font color="red">*</font>
       <font style="font-size:13px;font-family:'Microsoft YaHei'">病 历 号 :</font>&nbsp;
       <el-input  placeholder="病历号" style="width: 110px"  again="left" size="mini" v-model="keywords"
       @keyup.enter.native="searchClick">
       </el-input>
  </div>
  </el-col>

  <el-col :span="5">
  <div class="grid-content bg-purple">
      <el-button type="primary" icon="el-icon-search" size="mini" style="margin-left: 3px" @click="searchClick">搜索
      </el-button>
  </div>
  </el-col>
  </el-row>

  <br>
  <br>
  <font style="font-size:15px;font-family:'Microsoft YaHei'">
        患者信息确认</font>&nbsp;

<el-row :gutter="2">  
<el-col :span="4">
  <div class="grid-content bg-purple">
        <font style="font-size:13px;font-family:'Microsoft YaHei'">姓&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;名:</font>&nbsp;
       <el-input  placeholder="姓名" style="width: 110px"  again="left" size="mini" readonly 
       v-model="RealName" >
       </el-input>
  </div>
  </el-col>
  
  <el-col :span="7">
  <div class="grid-content bg-purple">
  <font style="font-size:13px;font-family:'Microsoft YaHei'">身份证号:</font>
   <el-input  placeholder="身份证号" style="width: 180px" size="mini" readonly 
   v-model="IDnumber" ></el-input>
  </div>
  </el-col>

  <el-col :span="9">
  <div class="grid-content bg-purple">
  <font style="font-size:13px;font-family:'Microsoft YaHei'">家庭住址:</font>
   <el-input  placeholder="家庭住址" style="width: 330px" size="mini" readonly
   v-model="HomeAddress" ></el-input>
  </div>
  </el-col>
</el-row>  

<!-- :data="categories"  -->  
<el-main class="gao_cate_mana_main">
<font style="font-size:15px;font-family:'Microsoft YaHei'">
        患者发票信息</font>&nbsp;
 <el-table
        ref="multipleTable"
        :data="tableData"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange" v-loading="loading">
       
        <el-table-column
          label="ID"
          prop="ID"
          width="120" align="left"  v-if='false' >
        </el-table-column>

        <el-table-column
          label="病历号"
          prop="CaseNumber"
          width="120" align="left">
        </el-table-column>

         <el-table-column
          label="发票号"
          prop="InvoiceNum"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="姓名"
          prop="RealName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="金额"
          prop="Money"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="发票状态"
          prop="State"
          width="120" align="left">
  <template slot-scope="scope">
              {{ scope.row.State|showScope }}
  </template>
        </el-table-column>
        
        <el-table-column
          label="看诊日期"
          prop="VisitDate"
          width="120" align="left">
        </el-table-column>
      <el-table-column label="操作" align="left">
          <template slot-scope="scope">
           <el-button
              size="mini"
              type="primary"
              @click="handleMakeup(scope.$index, scope.row)" icon="el-icon-sold-out">发票补打
            </el-button>
          </template>
        </el-table-column>
      </el-table>
</el-main>
    
<!-- 发票补打弹框---start -->
        <el-dialog title="发票补打(发票信息)" :visible.sync="dialogFormVisible" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
        
                <el-row :gutter="2">
                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">发票号(可修改):</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                        <el-input  placeholder="发票号" size="mini" v-model="InvoiceNum02"></el-input>
                       </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">病历号:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                         <el-form-item label="">
                            <el-input  placeholder="病历号" size="mini" v-model="CaseNumber02" readonly></el-input>
                         </el-form-item>
                    </div>
                    </el-col>
                </el-row>

                <el-row :gutter="2">
                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">患者姓名:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                            <el-input   placeholder="患者姓名" size="mini" v-model="RealName02" readonly></el-input>
                        </el-form-item>
                    </div>
                    </el-col>
                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'" >总额:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input  placeholder="总额" size="mini" v-model="Money02" readonly></el-input>
                      </el-form-item>
                    </div>
                    </el-col>
                </el-row>

                <el-row :gutter="2">
                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input  placeholder="ID02" size="mini" v-model="ID02" readonly 
                           v-if='false' ></el-input>
                      </el-form-item>
                    </div>
                    </el-col>
                 </el-row>
             </el-form>
            <br>
            <el-button type="warning" @click="dialogFormVisible = false" size="mini">取 消</el-button>
            <el-button type="primary" @click="updateInvoice()" size="mini">打 印</el-button>
        </el-dialog>
    <!--发票补打弹框---end -->
</div>
</template>

<script>
  import {postRequest}   from '../../utils/api'
  import {putRequest}    from '../../utils/api'
  import {deleteRequest} from '../../utils/api'
  import {getRequest}    from '../../utils/api'
  
  export default{
    methods: {
       searchClick(){
         var _this = this;
        if (this.keywords == null || this.keywords.length == 0) {
            _this.$message({
              type: 'info',
              message: '病历号不能为空!'
            });
          } 
          else {
             _this.loadBlogs(1, 100);//100 this.pageSize  暂时不分页
          }
      },
      loadBlogs(page, count){
        var _this = this;
        var url0 = "/neureg/register/get_register?CaseNumber=" + this.keywords ;
         //按病历号查询 
         getRequest(url0).then(resp=> {
            var json = resp.data;
            if (json == null || json.length == 0){
                this.RealName= "";
                this.IDnumber= "";
                this.HomeAddress = "";
            }
            else {
                this.RealName= json.realName;
                this.IDnumber= json.iDnumber;
                this.HomeAddress = json.homeAddress;
            }
        });
        //向服务器发 发票查询请求
        var url = "/neureg/invoice/invoice_query?page=" + page + "&count=" + count + "&keywords=" + this.keywords;
        getRequest(url).then(resp=> {
          _this.loading = false;
          if (resp.status == 200) {
            _this.tableData = resp.data.list;
            _this.totalCount = resp.data.totalCount;
          } else {
            _this.$message({type: 'error', message: '数据加载失败!'});
          }
        }, resp=> {
          _this.loading = false;
          if (resp.response.status == 403) {
            _this.$message({type: 'error', message: resp.response.data});
          } else {
            _this.$message({type: 'error', message: '数据加载失败!'});
          }
        }).catch(resp=> {
          //压根没见到服务器
          _this.loading = false;
          _this.$message({type: 'error', message: '数据加载失败!'});
        })
      },
      //翻页
      currentChange(currentPage){
        this.currentPage = currentPage;
        this.loading = true;
        this.loadBlogs(currentPage, this.pageSize);
      },
      
      handleSelectionChange(val) {
        this.selItems = val;
      },
     
      handleMakeup(index, row){
        let _this = this;
        _this.CaseNumber02 = row.CaseNumber;
        _this.InvoiceNum02= row.InvoiceNum;
        _this.RealName02= row.RealName;
        _this.Money02= row.Money;
        _this.SettleName02 = row.SettleName;
        _this.ID02 = row.ID;
        _this.dialogFormVisible = true;
        //console.log("BBBBBB");
      },
      updateInvoice(){
        var _this = this;
        if (this.InvoiceNum02 == null || this.InvoiceNum02.length == 0)
        {
            _this.$message({
              type: 'info',
              message: '发票编号不能为空!'
            });
        } 
        else {
        //向服务器发 发票补打请求
        putRequest("/neureg/invoice/invoice_makeup", {ID: this.ID02, InvoiceNum: this.InvoiceNum02}).then(resp=> {
              var json = resp.data;
              _this.$message({
                type: json.status,
                message: json.msg
              });
           }, resp=> {
              if (resp.response.status == 403) {
                _this.$message({
                  type: 'error',
                  message: resp.response.data
                });
              }
              _this.loading = false;
            }); 
            _this.loading = false;
            _this.dialogFormVisible = false;
            _this.searchClick();
            _this.refresh();
          }
      },
      
      refresh(){
        let _this = this;
        _this.loading = false;
      }
    },
    mounted: function () {
      this.tableData="";
    },
     filters:{
      showScope:function(scope){
        if(scope=='1'){
          return '正常';
        }else if(scope=='2'){
          return '作废';
        }else if(scope=='3'){
          return '未打印';
        }else if(scope=='4'){
          return '已打印';
        }else if(scope=='5'){
          return '已重打';
        }else if(scope=='6'){
          return '已补打';
        }else if(scope=='7'){
          return '冲红';
        }
      }
    },
    data(){
      return {
        cateName: '',
        sfzh: '',
        selItems: [],
        categories: [],
        loading: false,
        currentPage: 1,
        totalCount: 2,
        pageSize: 3,
        keywords: '',
        keywords2:'',
        ID02:'',
        dialogAddVisible: false,
        dialogFormVisible: false,
        tableData: [],
        tableData2: [],
      }
    }
  }
</script>