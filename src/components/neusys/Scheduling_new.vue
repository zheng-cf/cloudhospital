<template>
<gao-el-container>  
  
   <font style="font-size:13px;font-family:'Microsoft YaHei'">开始时间:</font>&nbsp;  
      <el-date-picker
            type="date"
              placeholder="选择日期" size="mini" style="width: 130px;" v-model="keywords"  value-format="yyyy-MM-dd">
      </el-date-picker>
      &nbsp;&nbsp;
      <font style="font-size:13px;font-family:'Microsoft YaHei'">结束时间:</font>&nbsp;  
      <el-date-picker
            type="date"
              placeholder="选择日期" size="mini" style="width: 130px;" v-model="keywords2" value-format="yyyy-MM-dd">
      </el-date-picker>

      <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-view" @click="searchClick()">查询医生排班</el-button>

      <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept()">新增排班规则</el-button>

      <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-view" @click="newDept2()">查询排班规则(生成排班计划)</el-button>

   <p></p>
    <!-- :data="categories"  --> 
      <el-table
        ref="multipleTable"
        :data="tableData3"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange" v-loading="loading">
        <el-table-column
          type="selection"
          width="55" align="left">
        </el-table-column>
        
        <el-table-column
          label="日期"
          prop="SchedDate"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="科室"
          prop="DeptName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="医生"
          prop="RealName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="午别"
          prop="Noon"
          width="120" align="left">
          
        </el-table-column>

        <el-table-column
          label="挂号级别"
          prop="RegistName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="挂号限额"
          prop="RegistQuota"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="ID"
          prop="ID"
          width="120" align="left" v-if='false'>
        </el-table-column>

        <!--
        <el-table-column label="操作" align="left">
          <template slot-scope="scope">
            <el-button type="primary"
              size="mini"
              @click="handleEdit(scope.$index, scope.row)">编辑
            </el-button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete(scope.$index, scope.row)">删除
            </el-button>
          </template>
        </el-table-column>
      
     
      <el-button type="danger" :disabled="this.selItems.length==0" style="margin-top: 10px;width: 100px;" size="mini"
                 @click="deleteAll" v-if="this.tableData3.length>0">批量删除
      </el-button>
      -->
 </el-table>
      <div>
      <el-row :gutter="2">
            <el-col :span="6">
            <el-button type="danger" :disabled="this.selItems.length==0" style="margin-top: 10px;width: 100px;" size="mini"
                 @click="deleteAll" v-if="this.tableData3.length>0">批量删除
            </el-button>
            </el-col>
            <el-col :span="5">
            <el-pagination
              background
              :page-size="pageSize" :current-page.sync="currentPage01"
              layout="prev, pager, next"
              :total="totalCount" @current-change="currentChange" v-show="this.tableData3.length>0">
            </el-pagination>
            </el-col>
      </el-row>
    </div>
  

<!-- 新增排班规则 弹框---start -->

  <el-dialog title="新增排班规则" :visible.sync="dialogFormVisible" width="1400px" align="center">
  
  <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
   <font style="font-size:13px;font-family:'Microsoft YaHei'">科室选择:</font>              
   <el-select  placeholder="科室" style="width: 110px" size="mini" v-model="keywords01">
         
          <el-option
              v-for="item in categories3"
              :key="item.ID"
              :label="item.DeptName"
              :value="item.ID">
          </el-option>
    </el-select>
    <!-- <font color="red">*</font>&nbsp;&nbsp;  -->

  <font style="font-size:13px;font-family:'Microsoft YaHei'">挂号级别:</font>
  <el-select  placeholder="挂号级别" style="width: 110px"
                     size="mini" v-model="keywords02">
             
              <el-option
                  v-for="item in categories4"
                       :key="item.ID"
                       :label="item.RegistName"
                       :value="item.ID">
                  </el-option>
              </el-select>
              <!-- <font color="red">*</font>&nbsp;&nbsp;  -->
  <!--
  &nbsp;&nbsp;
  <font style="font-size:13px;font-family:'Microsoft YaHei'">用户名:</font>
  <el-input  placeholder="用户名" style="width: 110px"  again="left" size="mini" v-model="keywords03">
  </el-input>
  -->
  <el-button type="primary" @click="searchClick2" size="mini">查 询</el-button>

  <el-button type="primary" icon="el-icon-refresh" size="mini" style="margin-left: 3px" @click="clear()">清空
  </el-button>

  <p></p>

  <el-table
    :data="tableData"
    border
    style="width: 100%"
    @selection-change="handleSelectionChange" v-loading="loading">
    
    <el-table-column
          type="selection"
          width="55" align="left">
    </el-table-column>
    <el-table-column
      prop="Id"
      label="Id"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="DeptId"
      label="DeptId"
      width="100" align="center" v-if='false'>
    </el-table-column>

     <el-table-column
      prop="UserName"
      label="用户名"
      width="100" align="center" v-if='false'>
    </el-table-column>

    <el-table-column
      prop="RealName"
      label="医生名称"
      width="100" align="center">
    </el-table-column>

    <el-table-column
      prop="X10"
      label="X10"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期一 上午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X10"  true-value="1" false-value="0" >
      </template>
    </el-table-column>


    <el-table-column
      prop="X11"
      label="X11"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期一 下午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X11"  true-value="1" false-value="0" >
      </template>
    </el-table-column>
    
    <el-table-column
      prop="X20"
      label="X20"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期二 上午"
      width="100" align="center" >
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X20"  true-value="1" false-value="0" >
      </template>
    </el-table-column>

    <el-table-column
      prop="X21"
      label="X21"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期二 下午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X21"  true-value="1" false-value="0" >
      </template>
    </el-table-column>

    <el-table-column
      prop="X30"
      label="X30"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期三 上午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X30"  true-value="1" false-value="0" >
      </template>
    </el-table-column>
    <el-table-column
      prop="X31"
      label="X31"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期三 下午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X31"  true-value="1" false-value="0" >
      </template>
    </el-table-column>


    <el-table-column
      prop="X40"
      label="X40"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期四 上午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X40"  true-value="1" false-value="0" >
      </template>
    </el-table-column>
    <el-table-column
      prop="X41"
      label="X41"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期四 下午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X41"  true-value="1" false-value="0" >
      </template>
    </el-table-column>


    <el-table-column
      prop="X50"
      label="X50"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期五 上午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X50"  true-value="1" false-value="0" >
      </template>
    </el-table-column>
    <el-table-column
      prop="X51"
      label="X51"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期五 下午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X51"  true-value="1" false-value="0" >
      </template>
    </el-table-column>


    <el-table-column
      prop="X60"
      label="X60"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期六 上午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X60"  true-value="1" false-value="0" >
      </template>
    </el-table-column>
    <el-table-column
      prop="X61"
      label="X61"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期六 下午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X61"  true-value="1" false-value="0" >
      </template>
    </el-table-column>


    <el-table-column
      prop="X70"
      label="X70"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期日 上午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X70"  true-value="1" false-value="0" >
      </template>
    </el-table-column>
    <el-table-column
      prop="X71"
      label="X71"
      width="100" align="center" v-if='false'>
    </el-table-column>
    <el-table-column
      prop="province"
      label="星期日 下午"
      width="100" align="center">
      <template slot-scope="scope">
        <input type="checkbox" v-model="tableData[scope.$index].X71"  true-value="1" false-value="0" >
      </template>
    </el-table-column>

<!-- 
    <el-table-column label="操作" align="left">
          <template slot-scope="scope">
            <el-button type="primary"
              size="mini"
              @click="hello(scope.$index, scope.row)">编辑
            </el-button>
          </template>
    </el-table-column>
-->
    
  </el-table>
  <p></p>
  
  &nbsp;&nbsp;&nbsp;&nbsp;
  <font style="font-size:13px;font-family:'Microsoft YaHei'">规则名称:</font>
  <el-input  placeholder="规则名称" style="width: 110px"  again="left" size="mini" v-model="gzmc">
  </el-input><font color="red">*</font>
 </el-form>
  
  <br>
  <el-button type="warning" @click="dialogFormVisible = false" size="mini">取 消</el-button>
  <el-button type="primary" v-if="this.tableData.length>0" size="mini"
   :disabled="this.selItems.length==0"  @click="gzSave">保 存</el-button>
</el-dialog>
<!-- 新增排班规则 弹框---end -->

<!-- 查询排班规则 弹框---start -->
  <el-dialog title="排班规则列表" :visible.sync="dialogAddVisible" width="850px" align="center">
        <font style="font-size:13px;font-family:'Microsoft YaHei'">科室选择:</font>              
        <el-select  placeholder="科室" style="width: 110px" size="mini" v-model="DeptId02">
                <!--
                <el-option label="全院" value="0"></el-option>
                <el-option label="骨外科" value="1"></el-option>
                <el-option label="神经内科" value="2"></el-option>
                <el-option label="儿科" value="3"></el-option>
                -->
                <el-option
                    v-for="item in categories3"
                    :key="item.ID"
                    :label="item.DeptName"
                    :value="item.ID">
                </el-option>
        </el-select>&nbsp;&nbsp;    

  <el-button type="primary" @click="searchClick3" size="mini">查询</el-button>

   <el-button type="primary" icon="el-icon-refresh" size="mini" style="margin-left: 3px" @click="clear2()">清空
  </el-button>
  
  <!-- :data="categories202" -->
   <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formAdd" class="demo-form-inline">
        
        <el-table
        ref="multipleTable"
        :data="tableData2"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange202" v-loading="loading">

        <el-table-column
          type="selection"
          width="55" align="left">
        </el-table-column>

        <el-table-column
          label="规则Id"
          prop="Id"
          width="120" align="left" v-if='false'>
        </el-table-column> 
        <el-table-column
          label="DeptId"
          prop="DeptId"
          width="120" align="left" v-if='false'>
        </el-table-column> 
        <el-table-column
          label="UserId"
          prop="UserId"
          width="120" align="left" v-if='false'>
        </el-table-column>   


        <el-table-column
          label="规则名称"
          prop="RuleName"
          width="120" align="left">
        </el-table-column>
        
        <el-table-column
          label="科室名称"
          prop="DeptName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="医生姓名"
          prop="RealName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="时间"
          prop="Week"
          width="150" align="left">
        </el-table-column>
<!--
      <el-table-column
          label="默认规则"
          prop="ff"
          width="120" align="left">
      </el-table-column>
-->

      </el-table>
      </el-form>
      <!--
      <el-button type="primary" @click="" size="mini" :disabled="this.selItems202.length==0">设为默认规则</el-button>
      -->
      <br><br>
      <font style="font-size:13px;font-family:'Microsoft YaHei'">开始时间:</font>&nbsp;  
      <el-date-picker
            type="date"
              placeholder="选择日期" size="mini" style="width: 130px;" v-model="ksrq" value-format="yyyy-MM-dd">
      </el-date-picker><font color="red">*</font>
      &nbsp;&nbsp;
      <font style="font-size:13px;font-family:'Microsoft YaHei'">结束时间:</font>&nbsp;  
      <el-date-picker
            type="date"
              placeholder="选择日期" size="mini" style="width: 130px;" v-model="jsrq" value-format="yyyy-MM-dd">
      </el-date-picker><font color="red">*</font>

      <el-button type="primary" @click="pbSave" size="mini" :disabled="this.selItems202.length==0">选取规则生成排班计划</el-button>
      <!--
      <el-button type="primary" @click="" size="mini">按默认规则生成排版计划</el-button>  
      -->
      <br><br>
      <el-button type="primary" @click="dialogAddVisible = false" size="mini">关闭</el-button>
      
      </el-dialog>
    <!-- 查询排班规则  弹框---end -->

</gao-el-container> 
</template>

<script>
  import {postRequest} from '../../utils/api'
  import {putRequest} from '../../utils/api'
  import {deleteRequest} from '../../utils/api'
  import {getRequest} from '../../utils/api'

  //const cityOptions = ['上海', '北京', '广州', '深圳'];

  export default{
    methods: {
    /*
    handleCheckAllChange(val) {
        this.checkedCities = val ? cityOptions : [];
        this.isIndeterminate = false;
      },
      handleCheckedCitiesChange(value) {
        let checkedCount = value.length;
        this.checkAll = checkedCount === this.cities.length;
        this.isIndeterminate = checkedCount > 0 && checkedCount < this.cities.length;
      },
      */
       //翻页相关
      currentChange(currentPage){
        this.currentPage01 = currentPage;
        this.loading = true;
        this.loadBlogs(currentPage, this.pageSize);
      },
      //排班查询 
      searchClick(){
         this.loadBlogs(1, this.pageSize);
      },
      //排班人员查询 
      searchClick2(){
        this.loadBlogs2(1, 100);//临时不翻页的情况
      },
      //排班规则查询 
      searchClick3(){
        this.loadBlogs3(1, 100);//临时不翻页的情况
      },
      //排班查询 相关  
      loadBlogs(page, count){
        var _this = this;
        
        var url = "/neusys/scheduling/all?page=" + page + "&count=" + count + "&keywords=" + this.keywords + "&keywords2=" + this.keywords2;
        getRequest(url).then(resp=> {
        if (resp.status == 200) {
             _this.loading = false;
            _this.tableData3 = resp.data.list;
            _this.totalCount = resp.data.totalCount;
            _this.currentPage01 = page ;
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
      //新增排班规则时的医生查询 相关  
      loadBlogs2(page, count){
        var _this = this;
        //01=/ucsys/user/all02   02=/neusys/user/all02
        var url = "/neusys/user/all02?page=" + page + "&count=" + count + "&keywords=" + this.keywords01 + "&keywords2=" + this.keywords02 + "&keywords3=" + this.keywords03;
        getRequest(url).then(resp=> {
        if (resp.status == 200) {
            _this.tableData = resp.data.list;
            _this.totalCount = resp.data.totalCount;
            //_this.currentPage01 = page ;
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
      //排班规则查询 相关  
      loadBlogs3(page, count){
        var _this = this;
        //01=/ucsys/rule/all   02=/neusys/rule/all
        var url = "/neusys/rule/all?page=" + page + "&count=" + count + "&keywords=" + this.DeptId02;
        getRequest(url).then(resp=> {
        if (resp.status == 200) {
            _this.tableData2 = resp.data.list;
            _this.totalCount = resp.data.totalCount;
            //_this.currentPage01 = page ;
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
      hello(index, row){
         //this.$confirm('确认 "' + row.X10 + '" ?', '提示', {
         this.$confirm('确认 "' + this.selItems[0].X10 + '" ?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        });
      },
      handleSelectionChange(val) {
        this.selItems = val;
      },
      handleSelectionChange202(val) {
        this.selItems202 = val;
      },
      clear2(){
         this.DeptId02="";
         this.ksrq="";
         this.jsrq="";
         //this.tableData2="";
      },
      clear(){
         this.keywords01="";
         this.keywords02="";
         this.keywords03="";
         this.gzmc="";
      },
      newDept(){
        let _this = this;
        _this.ch10="";
        _this.ch11="";
        _this.ch20="";
        _this.ch21="";
        _this.ch30="";
        _this.ch31="";
        _this.ch40="";
        _this.ch41="";
        _this.ch50="";
        _this.ch51="";
        _this.ch60="";
        _this.ch61="";
        _this.ch70="";
        _this.ch71="";
        _this.gzmc="";
        _this.selItems="";
        //_this.selItems=[];
        _this.tableData="";
        _this.keywords01="";
        _this.keywords02="";
        _this.keywords03="";

        _this.dialogFormVisible = true;
      },
      newDept2(){
        let _this = this;
        _this.tableData2="";
        _this.ksrq="";
        _this.jsrq="";
        _this.DeptId02="";
        _this.dialogAddVisible = true;
      },
      handleSelectionChange(val) {
        this.selItems = val;
      },
      gzSave(){
        var _this = this;
        
        if (this.gzmc == null || this.gzmc.length == 0) {
              _this.$message({
              type: 'info',
              message: '规则名称不能为空!'
            });
        } 
        else {
              this.$confirm('确认保存这 ' + this.selItems.length + ' 条规则吗?', '提示', {
              type: 'warning',
              confirmButtonText: '确定',
              cancelButtonText: '取消'
              }).then(()=> {
                  
              for (var i = 0; i < this.selItems.length; i++) {
                  
                   this.weeks[i]='';

                   if(this.selItems[i].X10=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';  
                   if(this.selItems[i].X11=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';    

                   if(this.selItems[i].X20=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';  
                   if(this.selItems[i].X21=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                     this.weeks[i]=this.weeks[i]+'0';    

                   if(this.selItems[i].X30=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';  
                   if(this.selItems[i].X31=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';      

                   if(this.selItems[i].X40=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';  
                   if(this.selItems[i].X41=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';    

                   if(this.selItems[i].X50=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';  
                   if(this.selItems[i].X51=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';   

                   if(this.selItems[i].X60=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';  
                   if(this.selItems[i].X61=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';       

                   if(this.selItems[i].X70=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';  
                   if(this.selItems[i].X71=='1')
                      this.weeks[i]=this.weeks[i]+'1';
                   else
                      this.weeks[i]=this.weeks[i]+'0';          
              }
              for (var i = 0; i < this.selItems.length; i++) {
                  _this.savaRule(this.selItems[i].Id, this.selItems[i].DeptId, this.weeks[i]);
              }
              _this.tableData="";
              _this.keywords01="";
              _this.keywords02="";
              _this.keywords03="";
              _this.gzmc="";
              
              }).catch(() => {
               //取消
              _this.loading = false;
              });
        }
      },
      savaRule(ids,dids,weeks){
        var _this = this;
        this.loading = true;
         //UserId: '100'
         //01=/ucsys/rule/add  02=/neusys/rule/add
         postRequest('/neusys/rule/add', {UserId: ids, RuleName: this.gzmc,DeptId:dids,Week:weeks}).then(resp=> {
            if (resp.status == 200) {
              var json = resp.data;
              _this.$message({type: json.status, message: json.msg});
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
      },
      pbSave(){
        var _this = this;
        
        if (this.ksrq == null || this.ksrq.length == 0
            ||this.jsrq == null || this.jsrq.length == 0) {
            
              _this.$message({
              type: 'info',
              message: '开始日期和结束日期 不能为空!'
            });
        } 
        else {

            for (var i = 0; i < this.selItems202.length; i++) {
                _this.savaPaiBan(this.ksrq,this.jsrq,this.selItems202[i].Id,this.selItems202[i].DeptId,this.selItems202[i].UserId,this.selItems202[i].Week);
            }
            _this.ksrq="";
            _this.jsrq="";
            _this.DeptId02="";
          }
      },
      
      savaPaiBan(ksrq,jsrq,ids,dids,uids,weeks){
        var _this = this;
        //this.loading = true;
        console.log("浏览器控制台输出：ksrq="+ksrq);
        console.log("浏览器控制台输出：jsrq="+jsrq);
        console.log("浏览器控制台输出：ids="+ids,);
        console.log("浏览器控制台输出：dids="+dids);
        console.log("浏览器控制台输出：uids="+uids);
        console.log("浏览器控制台输出：weeks="+weeks);
        console.log("====================================================");
        //01=/ucsys/scheduling/add   02=/neusys/scheduling/add
         postRequest('/neusys/scheduling/add', {ksrq:ksrq, jsrq:jsrq, UserId: uids, DeptId:dids, RuleId:ids, Week:weeks}).then(resp=> {
            if (resp.status == 200) {
              var json = resp.data;
              _this.$message({type: json.status, message: json.msg});
              //_this.ConstantTypeCode3 = '';
              //_this.ConstantTypeName3 = '';
              //_this.refresh();
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
              ids += selItems[i].ID + ",";
          }
          _this.deleteCate(ids.substring(0, ids.length - 1));
          //this.loading = true;
          this.searchClick();
        }).catch(() => {
          //取消
          _this.loading = false;
        });
      },      
      deleteCate(ids){
        var _this = this;
        //this.loading = true;
        //删除
        //01=/ucsys/scheduling/   02=/neusys/scheduling/
        deleteRequest("/neusys/scheduling/" + ids).then(resp=> {
          var json = resp.data;
          _this.$message({
            type: json.status,
            message: json.msg
          });
          //_this.refresh();
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

        //所在科室 
        getRequest("/neusys/department/all").then(resp=> {
            _this.categories3 = resp.data.list;
        }); 

        //挂号级别
        getRequest("/neusys/registLevel/all").then(resp=> {
            _this.categories4 = resp.data.list;
        }); 

        _this.searchClick()
        _this.loading = false;
       }
    },
    mounted: function () {
      this.loading = true;
      this.refresh();
    },

    data(){
     
      return {

        //checkAll: false,
        //checkedCities: ['上海', '北京'],
        //cities: cityOptions,
        //isIndeterminate: true,

        //翻页相关 { 
        totalCount: 2,
        currentPage01:1,
        pageSize: 10,
        //翻页相关 }

        gzmc: '',
        selItems: [],
        weeks: [],
        ksrq: '',
        jsrq: '',
        DeptId02: '',

        keywords: '',
        keywords2: '',

        keywords01: '',
        keywords02: '',
        keywords03: '',
        
        categories: [],
        categories202: [],

        categories3: [],
        categories4: [],
        selItems202: [],
        tableData2: [],
        tableData3: [],
        loading: false,
        dialogAddVisible: false,
        dialogFormVisible: false,

        ///*
        tableData: [{
          date: '霍去病',
          name: '20',
          province: '20',
          city: '100',
          address: '200',
          zip: 300},
          {
          date: '范无病',
          name: '20',
          province: '20',
          city: '100',
          address: '200',
          zip: 300},
          ],
        //*/  
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