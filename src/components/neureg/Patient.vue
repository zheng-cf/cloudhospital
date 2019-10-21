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
       @keyup.enter.native="searchClick" >
       </el-input>
  </div>
  </el-col>
  <el-col :span="5">
  <font style="font-size:13px;font-family:'Microsoft YaHei'">开始时间:</font>&nbsp;  
      <el-date-picker
            type="date"
              placeholder="选择日期" size="mini" style="width: 130px;" v-model="keywords2"  value-format="yyyy-MM-dd">
      </el-date-picker>
      &nbsp;&nbsp;
   </el-col>
   <el-col :span="5">
      <font style="font-size:13px;font-family:'Microsoft YaHei'">结束时间:</font>&nbsp;  
      <el-date-picker
            type="date"
              placeholder="选择日期" size="mini" style="width: 130px;" v-model="keywords3" value-format="yyyy-MM-dd">
      </el-date-picker>
    </el-col>
  <el-col :span="5">
  <div class="grid-content bg-purple">
      <el-button type="primary" icon="el-icon-search" size="mini" style="margin-left: 3px" @click="searchClick">搜索
      </el-button>
  </div>
  </el-col>
  </el-row>
<br><br>
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
        患者消费信息</font>&nbsp;
 <el-table
        ref="multipleTable"
        :data="tableData"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange" v-loading="loading">
        <!--
        <el-table-column
          type="selection"
          width="55" align="left">
        </el-table-column>
        -->
        
        <el-table-column
          label="ID"
          prop="ID"
          width="120" align="left" v-if='false'>
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
          label="消费名称"
          prop="Name"
          width="220" align="left">
        </el-table-column>

        <el-table-column
          label="单价"
          prop="Price"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="数量"
          prop="Amount"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="日期"
          prop="VisitDate"
          width="120" align="left">
        </el-table-column>

    </el-table>
</el-main>
    &nbsp;&nbsp;总笔数：{{zbs}} &nbsp;&nbsp;&nbsp;&nbsp;总金额： {{zje}}
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
        _this.zje=0;
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
        
        var url = "/neureg/patientCosts/patient_query?page=" + page + "&count=" + count + "&keywords=" + this.keywords + "&keywords2=" + this.keywords2 + "&keywords3=" + this.keywords3;
        getRequest(url).then(resp=> {
          _this.loading = false;
          if (resp.status == 200) {
            _this.tableData = resp.data.list;
            _this.zbs = resp.data.totalCount;
            //_this.totalCount = resp.data.totalCount;
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
        });

        //this.zje=_this.tableData.Price;
        var url2 = "/neureg/patientCosts/patient_sum?page=" + page + "&count=" + count + "&keywords=" + this.keywords + "&keywords2=" + this.keywords2 + "&keywords3=" + this.keywords3;
        getRequest(url2).then(resp=> {
          _this.loading = false;
          if (resp.status == 200) {
             _this.zje = resp.data.totalCount;
          } else {
            _this.$message({type: 'error', message: '数据加载失败!'});
          }
        });
        
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
        this.$prompt('请输入新名称', '编辑', {
          confirmButtonText: '更新',
          inputValue: row.personName,
          cancelButtonText: '取消'
        }).then(({value}) => {
          //value就是输入值
          if (value == null || value.length == 0) {
            _this.$message({
              type: 'info',
              message: '数据不能为空!'
            });
          } else {
            _this.loading = true;
            
          }
        });
      },
      handleDelete(index, row){
        let _this = this;

        _this.CaseNumber02 = row.CaseNumber;
        _this.InvoiceNum02= row.InvoiceNum;
        _this.RealName02= row.RealName;
        _this.Money02= row.Money;
        _this.SettleName02 = row.SettleName;
        _this.Id02 = row.Id;

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
        putRequest("/neureg/invoice/update02", {id: this.Id02, InvoiceNum: this.InvoiceNum02}).then(resp=> {
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
            _this.refresh();
          }
      },
      deleteCate(ids){
        var _this = this;
        this.loading = true;
      },
      refresh(){
        let _this = this;
      }
    },
    mounted: function () {
      this.loading = false;
      this.refresh();
    },
    data(){
      return {
        zbs: 0,
        zje: 0,
        cateName: '',
        cityName: '',
        nationName: '',
        sfzh: '',
        selItems: [],
        categories: [],
        loading: false,
        currentPage: 1,
        totalCount: 2,
        pageSize: 3,
        keywords: '',
        keywords2:'',
        keywords3:'',
        Id02:'',
        dialogAddVisible: false,
        dialogFormVisible: false,
          tableData: [{
          aa: '0000000003',
          bb: '张三',
          cc: '男',
          dd: '210103197802230987',
          ee: '2019-1-14',
          ff: '普通号',
          gg: '未看诊'},{
          aa: '0000000004',
          bb: '李四',
          cc: '女',
          dd: '210103197802230988',
          ee: '2019-1-14',
          ff: '普通号',
          gg: '未看诊'},
          ],
          tableData2: [{
          aa: '门诊普通发票',
          bb: '20190114001',
          cc: '20190114100',
          dd: '20190114035',
          ee: '在用',
          ff: '普通号',
          gg: '未看诊'},{
          aa: '门诊普通发票',
          bb: '20190114101',
          cc: '20190114200',
          dd: '',
          ee: '未用',
          ff: '普通号',
          gg: '未看诊'},
          ],
      }
    }
  }
</script>