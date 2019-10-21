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

<!-- :data="categories"   v-if='false' -->  
<el-main class="gao_cate_mana_main">
<font style="font-size:15px;font-family:'Microsoft YaHei'">
        患者交费信息</font>&nbsp;
 <el-table
        ref="multipleTable"
        :data="tableData"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange" v-loading="loading">
        
        <el-table-column
          type="selection"
          width="55" align="left">
        </el-table-column>
        
        <!-- v-if='false' -->
        <el-table-column
          label="prenID"
          prop="prenID"
          width="120" align="left" v-if='false' >
        </el-table-column>
        
        <el-table-column
          label="挂号员ID"
          prop="RegisterID"
          width="120" align="left" v-if='false' >
        </el-table-column>

        <el-table-column
          label="挂号记录ID"
          prop="RegistID"
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
          label="项目名称"
          prop="DrugsName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="单价"
          prop="DrugsPrice"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="数量"
          prop="Amount"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="开立时间"
          prop="PrescriptionTime"
          width="220" align="left">
        </el-table-column>

        <!-- v-if='false'  -->
        <el-table-column
          label="tabnum"
          prop="tabnum"
          width="220" align="left" v-if='false' >
        </el-table-column>

         <el-table-column
          label="PCID"
          prop="PCID"
          width="220" align="left" v-if='false' >
        </el-table-column>

        <el-table-column
          label="状态"
          prop="State"
          width="120" align="left">
  <template slot-scope="scope">
          {{ scope.row.State|showScope }}
  </template>
        </el-table-column>
       </el-table-column>
       </el-table>
       <br>
       <el-button type="primary" icon="el-icon-goods"  :disabled="this.selItems.length==0" style="margin-top: 10px;width: 100px;" size="mini"
                 @click="refundAll" v-if="this.tableData.length>0">退费确认
      </el-button>
</el-main>

<!-- 发票框---start -->
        <el-dialog title="发票信息(退费)" :visible.sync="dialogFormVisible" width="650px" align="center">
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
                        <font style="font-size:13px;font-family:'Microsoft YaHei'" >应退金额:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input  placeholder="应退金额" size="mini" v-model="Money02" readonly></el-input>
                      </el-form-item>
                    </div>
                    </el-col>
                </el-row>

                <el-row :gutter="2">
                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input  placeholder="prenID02" size="mini" v-model="prenID02" readonly v-if='false' ></el-input>
                      </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input  placeholder="RegistID" size="mini" v-model="RegistID02" readonly v-if='false' ></el-input>
                      </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input  placeholder="RegisterID" size="mini" v-model="RegisterID02" readonly v-if='false' ></el-input>
                      </el-form-item>
                    </div>
                    </el-col>
                    

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input  placeholder="send_str02" size="mini" v-model="send_str02" v-if='false' ></el-input>
                      </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input  placeholder="usr_str02" size="mini" v-model="usr_str02" v-if='false' ></el-input>
                      </el-form-item>
                    </div>
                    </el-col>

              </el-row>      
           </el-form>
           <br>
           <el-button type="warning" @click="dialogFormVisible = false" size="mini">取 消</el-button>
           <el-button type="primary" @click="updateInvoice()" size="mini">退 费</el-button>
        </el-dialog>
    <!--发票弹框---end -->
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
        if ( (this.keywords == null || this.keywords.length == 0)
             && (this.keywords2 == null || this.keywords2.length == 0)
           ) {
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
        this.tableData="";
   
        //获取患者信息
        var url2 = "/neureg/patientCosts/get_patient?keywords=" + this.keywords;
        getRequest(url2).then(resp=> {
          var json = resp.data;
            if (json == null || json.length == 0){
                this.HomeAddress = "";
                this.RealName= "";
                this.IDnumber= "";
            }
            else {
                this.HomeAddress = json.homeAddress;
                this.RealName= json.realName;
                this.IDnumber= json.iDnumber;
            }
        }); 

        //获取患者退款信息
        var url = "/neureg/patientCosts/patient_refund?page=" + page + "&count=" + count + "&keywords=" + this.keywords;
        getRequest(url).then(resp=> {
          _this.loading = false;
          if (resp.status == 200) {
            _this.tableData = resp.data.list;
            _this.totalCount = resp.data.totalCount;
            _this.zbs = resp.data.totalCount;
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
       
        //向服务器发 获取患者消费表的请求   3-已交费
        var url2 = "/neureg/patientCosts/patient_getTab?keywords=" + this.keywords+"&state1=5&state2=3";
        getRequest(url2).then(resp=> {
          _this.loading = false;
          if (resp.status == 200) {
             _this.tableInfo = resp.data.list;
          }
          });

      },
      //翻页
      currentChange(currentPage){
        this.currentPage = currentPage;
        this.loading = true;
        this.loadBlogs(currentPage, this.pageSize);
      },
      bnx(){
          this.$confirm('不可以修改的', '提示', {
          type: 'warning',
          confirmButtonText: '确定',
          cancelButtonText: '取消'
          });
          return false;
      },
      refundAll(){
        var _this = this;
        /*
         if(_this.zbs > this.selItems.length)
         {
                this.$confirm('只能全部退费', '提示', {
                  type: 'warning',
                  confirmButtonText: '确定',
                  cancelButtonText: '取消'
                });
                return;
         }
        */
        this.$confirm('确认退费这 ' + this.selItems.length + ' 条数据?', '提示', {
          type: 'warning',
          confirmButtonText: '确定',
          cancelButtonText: '取消'
        }).then(()=> {
          var selItems = _this.selItems;
          var sum_fee=0.0;
          var send_str="";
          var usr_str = "";

          for (var i = 0; i < selItems.length; i++) {
               
               sum_fee = sum_fee + selItems[i].DrugsPrice * selItems[i].Amount;

               send_str= ""+send_str + selItems[i].tabnum + "|" + selItems[i].PCID + ",";

               usr_str = ""+usr_str + selItems[i].DrugsName + "|" + selItems[i].DrugsPrice + "|" + selItems[i].Amount + "|" + selItems[i].RegistID + ",";
          }
          
          sum_fee = sum_fee.toFixed(2);
          //this.jf_ids=this.ids;
          _this.CaseNumber02 = selItems[0].CaseNumber;
          _this.RealName02= selItems[0].RealName;
          _this.Money02= sum_fee;
          _this.prenID02 = selItems[0].prenID;
          _this.RegisterID02 = selItems[0].RegisterID;
          _this.RegistID02 = selItems[0].RegistID;
          _this.zlje02="";
          _this.ssje02="";

          _this.send_str02 = send_str;

          _this.usr_str02 = usr_str;
          
          _this.dialogFormVisible=true;
        }).catch(() => {
          //取消
          _this.loading = false;
        });
      },
      handleSelectionChange(val) {
        this.selItems = val;
      },
      
      zhaoLing(){
           this.zlje02=this.ssje02 - this.Money02;
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
        if (this.InvoiceNum02 == null || this.InvoiceNum02.length == 0
           )
        {
            _this.$message({
              type: 'info',
              message: '发票号不能为空!'
            });
        } 
        else {

        /*
        //新加开始{
        var users = _this.tableData;
         var toStr = users.map(function ( users ) 
          { return users.prenID+"|"+ users.RegisterID+"|"+ users.DrugsPrice+"|"+ users.DrugsName+"|"+ users.Amount+"|"+ users.PrescriptionState+"|"+ users.RegistID +"|"+ users.CaseNumber +"|"+ users.RealName +"|"+ users.DrugsID;});

           var users2 =_this.tableInfo;

           var toStr2 = users2.map(function ( users2 ) 
              { return users2.prenID+"|"+ users2.tabnum;});

        //新加开始}
        */

        postRequest("/neureg/invoice/add318", {InvoiceNum: this.InvoiceNum02, CaseNumber02: this.CaseNumber02,Money: this.Money02, FeeType: this.FeeType02, InvoiceNum_old: this.jf_ids,RegisterID: this.RegisterID02,RegistID: this.RegistID02,prenID: this.prenID02,aaa: _this.usr_str02, bbb: _this.send_str02}).then(resp=> {
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

        /*
        //更新发票为作废   new add
        putRequest("/ucsys/invoice/update03", {id: this.InvoiceId02}).then(resp=> {
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
        postRequest("/ucsys/invoice/add03", {InvoiceNum: this.InvoiceNum02, CaseNumber02: this.CaseNumber02,Money: this.ssje02, ExpenseId: this.ExpenseId02, UserId: this.RegisterID02,SettleId:this.SettleId02,RegistId:this.RegistId02}).then(resp=> {
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
           
            var aa = _this.ids.substring(0, _this.ids.length - 1);
            _this.deleteCate(aa);
           */
            _this.loading = false;
            _this.dialogFormVisible = false;
            _this.refresh();
          }
      },
      deleteCate(ids){
        var _this = this;
        this.loading = true;
        //更新   putRequest
        //putRequest("/ucsys/patientCosts/update03" + ids).then(resp=> {
        deleteRequest("/ucsys/patientCosts/up/" + ids).then(resp=> {
          var json = resp.data;
          _this.$message({
            type: json.status,
            message: json.msg
          });
          _this.refresh();
        }, resp=> {
          _this.loading = false;
          if (resp.response.status == 403) {
            _this.$message({
              type: 'error',
              message: resp.response.data
            });
          } else if (resp.response.status == 500) {
            _this.$message({
              type: 'error',
              message: '删除失败!'
            });
          }
        })
      },
      refresh(){
        let _this = this;

         _this.ids="";
         this.tableData="";

         //向服务器发  获取最大发票号请求  结果+1
         getRequest("/neureg/invoice/bean01").then(resp=> {
            var json = resp.data;
            _this.InvoiceNum02 = parseInt(json.invoiceNum)+1;
         });

      }
    },
    mounted: function () {
      this.loading = false; 
      this.refresh();

      
      //this.$multipleTable.table.toggleRowSelection(this.tableData[1],true);
      //ref="multipleTable"
      //:data="tableData"
    },
     filters:{
      showScope:function(scope){
        if(scope=='1'){
          return '暂存';
        }else if(scope=='2'){
          return '已开立';
        }else if(scope=='3'){
          return '已交费';
        }else if(scope=='4'){
          return '已取药';
        }else if(scope=='5'){
          return '已退药';
        }else if(scope=='6'){
          return '已退费';
        }else if(scope=='0'){
          return '已作废';
        }
      }
    },
    data(){
      return {
        ids: '',
        zbs: 0,
        ssje02: '',
        zlje02: '',
        SettleId02: '',
        ExpenseId02: '',
        RegisterID02: '',
        RegistId02: '',
        InvoiceId02: '',
        
        send_str02: '',
        usr_str02: '',

        HomeAddress: '',
        RealName: '',
        IDnumber: '',

        cateName: '',
        cityName: '',
        nationName: '',
        sfzh: '',
        selItems: [],
        categories: [],
        categories2: [],
        loading: false,
        currentPage: 1,
        totalCount: 2,
        pageSize: 3,
        keywords: '',
        keywords2:'',
        keywords3:'',
        Id02:'',
        InvoiceNum02:'',
        dialogAddVisible: false,
        dialogFormVisible: false,
        tableData: [],
        tableInfo: [],
      }
    }
  }
</script>