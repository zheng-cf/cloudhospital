<template>
<div class="container">

<font style="font-size:15px;font-family:'Microsoft YaHei'">
        患者信息查询</font>&nbsp;
<el-row :gutter="2">
  <el-col :span="4">
  <div class="grid-content bg-purple">
       <font color="red">*</font>
       <font style="font-size:13px;font-family:'Microsoft YaHei'">病 历 号 :</font>&nbsp;
       <el-input  placeholder="病历号" style="width: 110px"  again="left" size="mini"
       v-model="keywords" @keyup.enter.native="searchClick" >
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
患者挂号信息</font>&nbsp;
 <el-table
        ref="multipleTable"
        :data="tableData"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange" v-loading="loading">
       
        <el-table-column
          label="ID"
          prop="ID"
          width="120" align="left" v-if='false' >
        </el-table-column>
        <el-table-column
          label="invID"
          prop="invID"
          width="120" align="left" v-if='false' >
        </el-table-column>
        <el-table-column
          label="FeeType"
          prop="FeeType"
          width="120" align="left" v-if='false' >
        </el-table-column>
        <el-table-column
          label="RegisterID"
          prop="RegisterID"
          width="120" align="left" v-if='false' >
        </el-table-column>

        <el-table-column
          label="InvoiceNum"
          prop="InvoiceNum"
          width="120" align="left" v-if='false' >
        </el-table-column>
        <el-table-column
          label="Money"
          prop="Money"
          width="120" align="left" v-if='false' >
        </el-table-column>
        
        <el-table-column
          label="RegistID"
          prop="RegistID"
          width="120" align="left" v-if='false' >
        </el-table-column>

        <el-table-column
          label="DeptID"
          prop="DeptID"
          width="120" align="left" v-if='false' >
        </el-table-column>

        <el-table-column
          label="Createteime"
          prop="Createteime"
          width="120" align="left" v-if='false' >
        </el-table-column>

        <el-table-column
          label="CreateOperID"
          prop="CreateOperID"
          width="120" align="left" v-if='false' >
        </el-table-column>

        <el-table-column
          label="pid"
          prop="pid"
          width="120" align="left" v-if='false' >
        </el-table-column>

        <el-table-column
          label="病历号"
          prop="CaseNumber"
          width="120" align="left">
        </el-table-column>
        <el-table-column
          label="姓名"
          prop="RealName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="身份证号"
          prop="IDnumber"
          width="170" align="left">
        </el-table-column>

        <el-table-column
          label="挂号日期"
          prop="VisitDate"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="挂号午别"
          prop="Noon"
          width="120" align="left">
        </el-table-column>


        

        <el-table-column
          label="看诊科室"
          prop="DeptName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="看诊状态"
          prop="VisitState"
          width="120" align="left">
  <template slot-scope="scope">
              {{ scope.row.VisitState|showScope }}
  </template>
        </el-table-column>

       <el-table-column label="操作" align="left">
          <template slot-scope="scope">
            <el-button
              size="mini"
              type="primary"
              @click="handleUpdateState(scope.$index, scope.row)" icon="el-icon-sold-out">退号
            </el-button>
          </template>
        </el-table-column>
      </el-table>
</el-main>
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
         if (_this.keywords == null || _this.keywords.length == 0) {
            _this.$message({
              type: 'info',
              message: '病历号不能为空!'
            });
          } 
          else {
             _this.loadBlogs(1, 100);//this.pageSize 100 暂时不分页
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
        //向服务器发 挂号信息查询请求
        var url = "/neureg/register/register_list?page=" + page + "&count=" + count + "&keywords=" + this.keywords + "&keywords2=" + this.keywords2;
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
      deleteAll(){
        var _this = this;
        this.$confirm('确认删除这 ' + this.selItems.length + ' 条数据?', '提示', {
          type: 'warning',
          confirmButtonText: '确定',
          cancelButtonText: '取消'
        }).then(()=> {
          var selItems = _this.selItems;
          var ids = '';
          for (var i = 0; i < selItems.length; i++) {
            ids += selItems[i].id + ",";
          }
          _this.deleteCate(ids.substring(0, ids.length - 1));
        }).catch(() => {
          //取消
          _this.loading = false;
        });
      },
      handleSelectionChange(val) {
        this.selItems = val;
      },
      handleEdit(index, row){
        var _this = this;
      },
      handleUpdateState(index, row){
        let _this = this;
        //if (row.VisitState == '已挂号') {

        if (row.VisitState == '1') {
          this.$confirm('确认退号 “' + row.RealName + '” 吗 ?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
           putRequest("/neureg/register/updateState", {ID: row.ID, InvoiceNum:row.InvoiceNum,Money:row.Money,RegisterID:row.RegisterID,RegistID:row.RegistID,invID:row.invID,FeeType:row.FeeType,DeptID:row.DeptID,Createteime:row.Createteime,CreateOperID:row.CreateOperID,pid:row.pid}).then(resp=> {
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
            _this.loadBlogs(1, 100);

        }).catch(() => {
          //取消
          _this.loading = false;
        });
     } else {
        _this.$message({
              type: 'info',
              message: '只有已挂号未看诊的患者才可以退号'
            });
       }
     },
      deleteCate(ids){
        let _this = this;
      },
      refresh(){
         let _this = this;
         _this.loading = false;
      }
    },
    mounted: function () {
       this.tableData="";
      //this.loading = true;
      //this.refresh();
    },
    filters:{
      showScope:function(scope){
        if(scope=='1'){
          return '已挂号';
        }else if(scope=='2'){
          return '医生接诊';
        }else if(scope=='3'){
          return '看诊结束';
        }else if(scope=='4'){
          return '已退号';
        }
      }
    },
    data(){
      return {
        cateName: '',
        selItems: [],
        categories: [],
        loading: false,
        currentPage: 1,
        totalCount: 2,
        pageSize: 10,
        keywords: '',
        keywords2:'',
        dialogAddVisible: false,
        dialogFormVisible: false,
        dialogAddVisible000: false,
        tableData: [],
      }
    }
  }
</script>
<style>
  .cate_mana_header {
    
    margin-top: 20px;
    padding-left: 5px;
    display: flex;
    justify-content: flex-start;
  }

  .cate_mana_main {
    /*justify-content: flex-start;*/
    display: flex;
    flex-direction: column;
    padding-left: 5px;
   
    margin-top: 20px;
    padding-top: 10px;
  }
</style>