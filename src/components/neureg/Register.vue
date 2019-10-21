<template>
<gao-el-container>
       <!--  readonly
       <el-button type="primary" icon="el-icon-view" size="mini" style="margin-left: 3px"
         @click="dialogAddVisible = true">门诊挂号发票</el-button>&nbsp;
       -->
       <font color="red">*</font>
       <font style="font-size:13px;font-family:'Microsoft YaHei'">发票号:</font>&nbsp;
       <el-input  placeholder="发票号" style="width: 110px"  again="left" size="mini" v-model="InvoiceNumber" >
       </el-input>
       </el-select>
        <!--
        <el-button type="primary" icon="el-icon-edit" size="mini" style="margin-left: 3px" @click="">更新发票号
        </el-button>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        -->
        <el-button type="primary" icon="el-icon-edit-outline" size="mini" style="margin-left: 3px" @click="addNewReg()">挂号
        </el-button>

        <el-button type="primary" icon="el-icon-refresh" size="mini" style="margin-left: 3px" @click="clear()">清空
        </el-button>
   
<br><br>
<font style="font-size:15px;font-family:'Microsoft YaHei'">
        挂号信息</font>&nbsp;
<el-row :gutter="2">
  <el-col :span="4">
  <div class="grid-content bg-purple">
       <font color="red">*</font>
       <font style="font-size:13px;font-family:'Microsoft YaHei'">病 历 号 :</font>&nbsp;
       <el-input  placeholder="输入病历号" style="width: 110px"  again="left" size="mini" v-model="CaseNumber" @blur="blhcx()">
       </el-input>
       
  </div></el-col>
  <el-col :span="4">
  <div class="grid-content bg-purple">
       <font color="red">*</font>
       <font style="font-size:13px;font-family:'Microsoft YaHei'">姓&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;名:</font>&nbsp;
       <el-input  placeholder="输入姓名" style="width: 110px"  again="left" size="mini" v-model="RealName">
       </el-input>
       
  </div></el-col>
  <el-col :span="4">
  <div class="grid-content bg-purple">
      <font color="red">*</font>
      <font style="font-size:13px;font-family:'Microsoft YaHei'">性&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;别:</font>&nbsp;
         <el-select  placeholder="选择性别" style="width: 110px" size="mini" v-model="Gender" >
           <el-option
                    v-for="item in categories2"
                    :key="item.ID"
                    :label="item.ConstantName"
                    :value="item.ID">
            </el-option>
         </el-select>
         
  </div>
  </el-col>
  <el-col :span="7">
  <div class="grid-content bg-purple">
    <font color="red">*</font>
    <font style="font-size:13px;font-family:'Microsoft YaHei'">年&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;龄:</font>&nbsp;
         <el-input  placeholder="输入年龄" style="width: 110px"  again="left" size="mini" v-model="Age">
         </el-input>
         <font color="red">*</font>
         <el-select  placeholder="选择岁" style="width: 110px" size="mini" v-model="AgeType">
            <el-option label="岁" value="岁"></el-option>
            <el-option label="月" value="月"></el-option>
            <el-option label="天" value="天"></el-option>
         </el-select>
  </div>
  </el-col>
 </el-row>

<el-row :gutter="2">
  <el-col :span="4">
  <div class="grid-content bg-purple">
      <font style="font-size:13px;font-family:'Microsoft YaHei'">出生日期:</font>
         <el-date-picker
                      v-model="BirthDate"
                      type="date"
                      placeholder="出生日期"  size="mini" style="width: 124px" value-format="yyyy-MM-dd">
         </el-date-picker>
  </div>
  </el-col>

  <el-col :span="7">
  <div class="grid-content bg-purple">
  <font style="font-size:13px;font-family:'Microsoft YaHei'">身份证号:</font>&nbsp;&nbsp;&nbsp;
   <el-input  placeholder="在此输入身份证号" style="width: 230px" size="mini" v-model="IDnumber"></el-input>
  </div>
  </el-col>
  <el-col :span="7">
  <div class="grid-content bg-purple">
      <font style="font-size:13px;font-family:'Microsoft YaHei'">家庭住址:</font>&nbsp;
       <el-input  placeholder="在此输入家庭住址" style="width: 250px"  again="left" size="mini" v-model="HomeAddress">
       </el-input>
  </div>
  </el-col>
</el-row>

<el-row :gutter="2">
<el-col :span="4">
  <div class="grid-content bg-purple">
    <font color="red">*</font>
    <font style="font-size:13px;font-family:'Microsoft YaHei'">结算类别:</font>&nbsp;   
         <el-select  placeholder="结算类别" style="width: 100px" size="mini" v-model="SettleID">
              <el-option
                    v-for="item in categories4"
                    :key="item.ID"
                    :label="item.SettleName"
                    :value="item.ID">
              </el-option>
         </el-select>
  </div>
  </el-col>
  <el-col :span="5">
     <div class="grid-content bg-purple">
     <font color="red">*</font>
     <font style="font-size:13px;font-family:'Microsoft YaHei'">看诊日期:
     <el-date-picker
                  v-model="VisitDate"
                  type="date"
                  placeholder="看诊日期"  size="mini" style="width: 125px" value-format="yyyy-MM-dd"  @change="kzrq_select()">
     </el-date-picker></font>
     </div>
  </el-col>
  <el-col :span="4">
  <div class="grid-content bg-purple">
    <font color="red">*</font>
    <font style="font-size:13px;font-family:'Microsoft YaHei'">午别:</font>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp   
         <el-select  placeholder="午别" style="width: 90px" size="mini" v-model="Noon" @change="Noon_select()">
          <el-option
                    v-for="item in categories5"
                    :key="item.Noon"
                    :label="item.Noon"
                    :value="item.Noon">
          </el-option> 
         </el-select>
  </div>
  </el-col>

  <el-col :span="4">
     <div class="grid-content bg-purple">
     <font color="red">*</font>
     <font style="font-size:13px;font-family:'Microsoft YaHei'">挂号科室:</font>&nbsp;
     <el-select  placeholder="挂号科室" style="width: 108px" size="mini" v-model="DeptID" @change="Dept_select()">
         <el-option
                    v-for="item in categories7"
                    :key="item.DeptID"
                    :label="item.DeptName"
                    :value="item.DeptID">
          </el-option> 
     </el-select>
     </div>
  </el-col>
  </el-row>
<el-row :gutter="2">
  <el-col :span="4">
     <div class="grid-content bg-purple">
     <font color="red">*</font>
     <font style="font-size:13px;font-family:'Microsoft YaHei'">号&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;别:</font>&nbsp;
     
     <el-select  placeholder="号别" style="width: 100px" size="mini" v-model="RegistLeID" @change="RegistLe_select()">
          <el-option
                    v-for="item in categories6"
                    :key="item.did"
                    :label="item.RegistName"
                    :value="item.did">
          </el-option> 
     </el-select>
     </div>
  </el-col>

  <el-col :span="4">
     <div class="grid-content bg-purple">
     <font color="red">*</font>
     <font style="font-size:13px;font-family:'Microsoft YaHei'">看诊医生:</font>
     <el-select  placeholder="看诊医生" style="width: 110px" size="mini" v-model="UserID"  @change="user_select()">
         <el-option
                    v-for="item in categories8"
                    :key="item.ID"
                    :label="item.RealName"
                    :value="item.ID">
          </el-option> 
     </el-select>
     </div>
  </el-col>

  <el-col :span="4">
     <div class="grid-content bg-purple">
     <font style="font-size:13px;font-family:'Microsoft YaHei'">初始号额:</font>
      <el-input  placeholder="初始号额" style="width: 110px"  again="left" size="mini" v-model="cshe" readonly>
       </el-input>
     </div>
  </el-col>

  <el-col :span="4">
     <div class="grid-content bg-purple">
     <font style="font-size:13px;font-family:'Microsoft YaHei'">已用号额:</font>
      <el-input  placeholder="已用号额" style="width: 110px"  again="left" size="mini" v-model="yyhe" readonly>
       </el-input>
     </div>
  </el-col>
 </el-row>

<el-row :gutter="2">
  <el-col :span="4">
  <div class="grid-content bg-purple">
   
  </div>
  </el-col>
  <el-col :span="4">
  <div class="grid-content bg-purple">
  
  </div>
  </el-col>
  <el-col :span="4">
  <div class="grid-content bg-purple">
   
   <font style="font-size:13px;font-family:'Microsoft YaHei'">病历本</font>
   
   <input type="checkbox" name=""  v-model="IsBook"  true-value="1" false-value="0" 
        @change="book_select()" 
   />
  </div>
  </el-col>
  
  <el-col :span="4"><div class="grid-content bg-purple">
      <font color="red">*</font>
      <font style="font-size:13px;font-family:'Microsoft YaHei'">应收金额:</font>&nbsp;
               <el-input  placeholder="应收金额" style="width: 100px"  again="left" size="mini" v-model="Fee" readonly >
               </el-input>
      </div>
  </el-col>

  <el-col :span="4">
  <div class="grid-content bg-purple">
    <font color="red">*</font>
    <font style="font-size:13px;font-family:'Microsoft YaHei'">收费方式:</font>&nbsp;   
         <el-select  placeholder="收费方式" style="width: 100px" size="mini" v-model="FeeType">
             <el-option
                    v-for="item in categories3"
                    :key="item.ID"
                    :label="item.ConstantName"
                    :value="item.ID">
              </el-option>
         </el-select>
  </div>
  </el-col>
</el-row>

</gao-el-container>
</template>

<script>
  import {postRequest} from '../../utils/api'
  import {putRequest} from '../../utils/api'
  import {deleteRequest} from '../../utils/api'
  import {getRequest} from '../../utils/api'

  import {uploadFileRequest} from '../../utils/api'
  // Local Registration
  import {mavonEditor} from 'mavon-editor'
  // 可以通过 mavonEditor.markdownIt 获取解析器markdown-it对象
  import 'mavon-editor/dist/css/index.css'
  import {isNotNullORBlank} from '../../utils/utils'

  export default{
  mounted: function () {

      this.RegisterID = sessionStorage.getItem("useID");
      this.loading = true;
      this.refresh();
      this.getCategories();
      this.getCategories2();
      var from = this.$route.query.from;
      this.from = from;
      var _this = this;
    },
    components: {
      mavonEditor
    },
    methods: {
      addNewReg(){
        var _this = this;
        if (this.CaseNumber == null || this.CaseNumber.length == 0
           || this.RealName == null || this.RealName.length == 0
           || this.Gender == null || this.Gender.length == 0
           || this.Age == null || this.Age.length == 0
           || this.AgeType == null || this.AgeType.length == 0
           || this.SettleID == null || this.SettleID.length == 0
           || this.VisitDate == null || this.VisitDate.length == 0
           || this.Noon == null || this.Noon.length == 0
           || this.RegistLeID == null || this.RegistLeID.length == 0
           || this.DeptID == null || this.DeptID.length == 0
           || this.UserID == null || this.UserID.length == 0
           || this.Fee == null || this.Fee.length == 0
           || this.InvoiceNumber == null || this.InvoiceNumber.length == 0
           || this.FeeType == null || this.FeeType.length == 0
          )  //|| this.IsBook == null || this.IsBook.length == 0 
        {
            _this.$message({
              type: 'info',
              message: '带红星项 不能为空!'
            });
        } 
        else {
            this.loading = true;

            this.syhe = this.cshe - this.yyhe;
            
            console.log("this.syhe="+this.syhe);

        if (this.syhe <= 0)
        {
             _this.$message({
              type: 'info',
              message: '该医生号额已满!'
            });
        }
        else{
             
             if (this.IsBook == null || this.IsBook.length == 0 )
                 this.IsBook=0;

             if (this.BirthDate == null || this.BirthDate.length == 0 )
                 this.BirthDate='1900-1-1';    

             //向服务器发挂号请求
             postRequest('/neureg/register/regist', {RealName: this.RealName, Gender: this.Gender, IDnumber: this.IDnumber, BirthDate: 
             this.BirthDate, Age: this.Age, AgeType: this.AgeType, HomeAddress: this.HomeAddress, CaseNumber: this.CaseNumber, VisitDate: this.VisitDate, Noon: this.Noon, DeptID: this.DeptID, UserID: this.UserID, RegistLeID: this.RegistLeID, SettleID: this.SettleID, IsBook: this.IsBook, InvoiceNumber: this.InvoiceNumber, RegisterTime: this.RegisterTime, RegisterID: this.RegisterID, Fee: this.Fee,FeeType:this.FeeType}).then(resp=> {

             if (resp.status == 200) {
                var json = resp.data;
                _this.$message({type: json.status, message: json.msg});
                _this.personName = '';
                this.clear();
                _this.refresh();
              }
              _this.loading = false;
            }, resp=> {
              if (resp.response.status == 403) {
                _this.$message({
                  type: 'error',
                  message: resp.response.data
                });
              }
              
              _this.loading = false;
            });
         }
      }},
      clear(){
          
          this.Gender= 71;//71 ""
          this.AgeType= "岁";
          this.FeeType=51; 
          this.SettleID=1;

          this.RealName ="";
          this.Age ="";
          this.BirthDate ="";
          this.HomeAddress ="";
          this.VisitDate ="";
          
          this.Noon ="";
          this.RegistLeID ="";
          this.UserID ="";
          this.IsBook ="";
          this.Fee ="";
          this.IDnumber="";
          this.InvoiceNum="";
          this.DeptID="";
          this.Fee=0.0;
         
          this.cshe="";
          this.yyhe="";
          this.syhe="";
      },
      //1查午别  根据排班日期(看诊日期)查午别
      kzrq_select()
      {
          var _this = this;

          _this.DeptID="";//科室
          _this.RegistLeID="";//号别
          _this.UserID ="";//医生
          _this.Noon="";//午别
          _this.cshe="";     //初始号额', 
          _this.yyhe="";     //已用号额', 
          _this.IsBook ="";
          _this.Fee=0.0;

          _this.categories5 ="";//午别
          
          //按排班日期(看诊日期)查午别
          var url05 = "/neusys/scheduling/get_noon?keywords=" +  this.VisitDate;
          getRequest(url05).then(resp=> {
            _this.categories5 = resp.data.list;
          });
      },
      //2查科室 根据日期和午别查科室
      Noon_select(){
          var _this = this;
          _this.categories7 ="";//科室
          _this.DeptId="";
          //根据日期和午别查科室   版本1=ucsys  版2=neusys
          var url03 = "/neusys/scheduling/get_dept?keywords=" +  this.VisitDate +"&keywords2=" + this.Noon;
          getRequest(url03).then(resp=> {
            _this.categories7 = resp.data.list;
          });
      },
      //3查号别 根据日期 午别 科室 查号别   
      Dept_select(){
          var _this = this;
          _this.categories6 ="";//号别
          _this.RegistLeId="";
          //根据日期 午别 科室 查号别
          var url04 = "/neusys/scheduling/get_regist_level?keywords=" +  this.VisitDate + "&keywords2=" + this.Noon +"&keywords3=" + this.DeptId;
          getRequest(url04).then(resp=> {
            _this.categories6 = resp.data.list;
          });
      },
      //4查医生  根据日期 午别 科室 号别 查医生
      RegistLe_select(){
          var _this = this;
          _this.categories8 ="";//医生
          _this.UserID ="";
          
          var url02 = "/neusys/scheduling/get_doc?keywords=" +  this.VisitDate + "&keywords2=" + this.Noon +"&keywords3=" + this.DeptID +"&keywords4=" + this.RegistLeID;
          getRequest(url02).then(resp=> {
            _this.categories8 = resp.data.list;
          });
      },
      //5查已用挂号额度  6查挂号额度   7查挂号费
      user_select()
      {
        var _this = this;

        if (this.Noon == null || this.Noon.length == 0)
        {
            this.UserID ="";
            _this.$message({
              type: 'info',
              message: '午别不能为空!'
            });
        } 
        else {
          //向服务器发 获取已用挂号额度的请求 
          var a_url = "/neureg/register/regist_num?keywords=" + this.VisitDate + "&keywords2="+ this.Noon +"&keywords3="+ this.UserID;
          getRequest(a_url).then(resp=> {
            _this.yyhe = resp.data;
          });

          //向服务器发 获取挂号额度   
          var b_url = "/neusys/scheduling/get_regist_quota?keywords=" + this.VisitDate + "&keywords2="+ this.Noon +"&keywords3="+ this.UserID;
          getRequest(b_url).then(resp=> {
            _this.cshe = resp.data;
          });

          //向服务器发 获取挂号费   
          var c_url = "/neusys/scheduling/get_regist_fee?keywords=" + this.VisitDate + "&keywords2="+ this.Noon +"&keywords3="+ this.UserID;
          getRequest(c_url).then(resp=> {
            _this.Fee = resp.data;
          });
        }
      },
      book_select(){
         if(this.Fee == 0){
            this.IsBook ="";
            return;
         }
         if(this.IsBook ==1)
            this.Fee=parseFloat(this.Fee) + 1;
         else
            this.Fee=parseFloat(this.Fee) - 1;
      },
      //按病历号查询患者信息  版本1=ucsys  版2=neureg
      blhcx(){
         var url = "/neureg/register/get_register?CaseNumber=" + this.CaseNumber ;
         //按病历号查询 
         getRequest(url).then(resp=> {
            var json = resp.data;
            if (json == null || json.length == 0){
                this.HomeAddress = "";
                this.RealName= "";
                this.IDnumber= "";
                this.Gender= 71;//71 ""
                this.AgeType= "岁";
                this.Age= "";
                this.BirthDate ="";
            }
            else {
                this.HomeAddress = json.homeAddress;
                this.RealName= json.realName;
                this.IDnumber= json.iDnumber;
                this.Gender= parseInt(json.gender);
                this.Age= json.age;
                this.AgeType= json.ageType;//parseInt(json.ageType)
            }
        });
      },
      refresh(){
        let _this = this;

        //向服务器发 获取 最大病例号 +1  
        getRequest("/neureg/register/max_casenumber").then(resp=> {
            var json = resp.data;
            _this.CaseNumber = parseInt(json.caseNumber)+1;
        });
       
        //向服务器发 获取 最大发票号 +1
        getRequest("/neureg/invoice/bean01").then(resp=> {
            var json = resp.data;
            _this.InvoiceNumber = parseInt(json.invoiceNum)+1;
        });

        //向服务器发 获取性别  常量数据
        getRequest("/neusys/constantItem/listByType?keywords=Gender").then(resp=> {
            _this.categories2 = resp.data.list;
        });

        //向服务器发 获取收费方式  常量数据
        getRequest("/neusys/constantItem/listByType?keywords=FeeType").then(resp=> {
            _this.categories3 = resp.data.list;
        });

        //向服务器发 获取结算类别   常量数据
        getRequest("/neusys/settleCategory/list").then(resp=> {
            _this.categories4 = resp.data.list;
        });
      }
    },

    data(){
      return {
        CaseNumber: '',     //病历号
        RealName: '',       //真实姓名
        Gender: 71,         //性别 71=男  72=女
        IDnumber: '',       //身份证号',
        BirthDate: '',      //出生日期',
        Age: '',            //年龄',
        AgeType: '岁',      //年龄类型',  岁
        HomeAddress: '',    //家庭住址',
        VisitDate: '',      //本次看诊日期', 2019-2-1 ???
        Noon: '',           //午别', 
        DeptID: '',         //本次挂号科室ID',
        UserID: '',         //本次挂号医生ID',
        RegistLeID: '',     //本次挂号级别ID',
        IsBook: '0',        //病历本要否',
        InvoiceNumber: '',  //发票号码',  
        RegisterTime: '',   //挂号时间',
        RegisterID: '',     //挂号员ID',
        Fee: '0',           //应收金额', 
        FeeType: 51,        //收费方式 51=现金 52=医保卡  
        SettleID: 1,        //结算类别ID', 1=自费 2=市医保
        cshe: '',           //初始号额', 
        yyhe: '',           //已用号额', 
        syhe: '',           //剩余号额', 
        loading: false,
        dialogAddVisible: false,

        categories2: [],
        categories3: [],
        categories4: [],
        categories5: [],
        categories6: [],
        categories7: [],
        categories8: [],
      }
    }
  }
</script>
<style>
</style>