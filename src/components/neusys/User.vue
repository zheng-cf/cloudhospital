<template>
<gao-el-container>
  <el-row :gutter="2">
      <el-col :span="4">
      <div class="grid-content bg-purple">
           <el-input
        placeholder="请输入登录名或姓名" size="mini"
        v-model="keywords" style="width: 170px;">
      </el-input>
      </div>
      </el-col>

      <el-col :span="3">
      <div class="grid-content bg-purple">
           <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-view" @click="searchClick">查询用户</el-button>
      </div>
      </el-col>

      <el-col :span="3">
      <div class="grid-content bg-purple">
           <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept()">新增用户</el-button>
      </div>
      </el-col>

      <!--
      <el-col :span="3">
      <div class="grid-content bg-purple">
         <el-upload
            class="upload-demo"
            action="https://jsonplaceholder.typicode.com/posts/"
            :on-preview="handlePreview"
            :on-remove="handleRemove"
            :before-remove="beforeRemove"
            multiple
            :limit="3"
            :on-exceed="handleExceed"
            :file-list="fileList"
            >

            <el-button size="mini" type="primary" icon="el-icon-sold-out">导入用户</el-button>
          </el-upload>
      </div>
      -->
      </el-col>
  </el-row>
    <p></p>
    <!-- :data="categories"  -->  
    <gao-el-main class="cate_mana_main">
      <el-table
        ref="multipleTable"
        :data="tableData"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange" v-loading="loading">
        <el-table-column
          type="selection"
          width="55" align="left" v-if='false' >
        </el-table-column>
        <el-table-column
          label="登录名"
          prop="UserName"
          width="120" align="left">
        </el-table-column>
        <el-table-column
          label="真实姓名"
          prop="RealName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="用户类别"
          prop="UseType"
          width="120" align="left">
  <template slot-scope="scope">
        {{ scope.row.UseType|showScope }}
  </template>      
        </el-table-column>
         <el-table-column
          label="用户类别2"
          prop="uid"
          width="120" align="left" v-if='false'>
        </el-table-column>

        <el-table-column
          label="医生职称"
          prop="ConstantName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="医生职称2"
          prop="dtid"
          width="120" align="left" v-if='false'>
        </el-table-column>

        <el-table-column
          label="所在科室"
          prop="DeptName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="所在科室2"
          prop="dpid"
          width="120" align="left" v-if='false'>
        </el-table-column>

        <el-table-column
          label="挂号级别"
          prop="RegistName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="挂号级别2"
          prop="reid"
          width="120" 
          align="left"
          v-if='false'
          >
        </el-table-column>

        <el-table-column
          label="Password"
          prop="Password"
          width="120" align="left" v-if='false'>
        </el-table-column>

        
        <el-table-column
          label="IsScheduling"
          prop="IsScheduling"
          width="120" align="left"  v-if='false'>
        </el-table-column>

        <el-table-column
          label="ID"
          prop="ID"
          width="120" 
          align="left" 
          v-if='false'
          >
        </el-table-column>

        <!-- v-if='false'  -->
       
        <el-table-column label="操作" align="left">
          <template slot-scope="scope">
            <el-button type="primary"
              size="mini"
              @click="handleEdit2(scope.$index, scope.row)">编辑
            </el-button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete(scope.$index, scope.row)">删除
            </el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-button type="danger" :disabled="this.selItems.length==0" style="margin-top: 10px;width: 100px;" size="mini"
                 @click="deleteAll" v-if="this.categories.length>0">批量删除
      </el-button>
    </gao-el-main>

<!-- 新增用户弹框---start -->
        <el-dialog title="新增用户" :visible.sync="dialogFormVisible" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
                
                <el-row :gutter="2">
                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">登录名:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input v-model="UserName02" placeholder="登录名" size="mini"></el-input>
                      </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">密码:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                         <el-form-item label="">
                            <el-input v-model="Password02" placeholder="密码" size="mini"></el-input>
                         </el-form-item>
                    </div>
                    </el-col>
                </el-row>

                <el-row :gutter="2">
                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">真实姓名:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                        <el-input v-model="RealName02" placeholder="真实姓名" size="mini"></el-input>
                       </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">所在科室:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                         <el-form-item label="">
                          <el-select  placeholder="所在科室" 
                           size="mini" v-model="DeptID02" >
                           <el-option
                                  v-for="item in categories3"
                                  :key="item.ID"
                                  :label="item.DeptName"
                                  :value="item.ID">
                            </el-option>
                            </el-select> 
                          </el-form-item>
                    </div>
                    </el-col>
                </el-row>

                <el-row :gutter="2">
                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">用户类别:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-select  placeholder="用户类别" 
                           size="mini" v-model="UseType02" >
                               <el-option label="医院管理员" value="1" key="1"></el-option>
                               <el-option label="挂号收费员" value="2" key="2"></el-option>
                               <el-option label="门诊医生"   value="3" key="3"></el-option>
                               <el-option label="医技医生"   value="4" key="4"></el-option>
                               <el-option label="药房操作员" value="5" key="5"></el-option>
                               <el-option label="财务管理员" value="6" key="6"></el-option>
                        </el-select>
                      </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">医生职称:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                         <el-form-item label="">
                          <el-select  placeholder="医生职称" 
                           size="mini"  v-model="DocTitleID02" >
                             <el-option
                            v-for="item in categories1"
                            :key="item.ID"
                            :label="item.ConstantName"
                            :value="item.ID">
                          </el-option>
                          </el-select>
                         </el-form-item>
                    </div>
                    </el-col>
                </el-row>
                
                <el-row :gutter="2">

                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">挂号级别:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                         <el-form-item label="">
                          <el-select  placeholder="挂号级别" 
                           size="mini"  v-model="RegistLeID02" >
                          <el-option
                            v-for="item in categories4"
                            :key="item.ID"
                            :label="item.RegistName"
                            :value="item.ID">
                          </el-option>
                     </el-select> 
                    </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="5">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">是否参与排班:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                        <el-form-item label="">
                        <el-radio v-model="IsScheduling02" label="是">是</el-radio>
                        <el-radio v-model="IsScheduling02" label="否">否</el-radio>
                        </el-form-item>
                    </div>
                    </el-col>
                </el-row>
            </el-form>
            <br>
            <el-button type="warning" @click="dialogFormVisible = false" size="mini">取 消</el-button>
            <el-button type="primary" @click="addNewCate" size="mini">保 存</el-button>
            <el-button type="primary" @click="clear" size="mini">清 空</el-button>
           
        </el-dialog>
<!-- 新增用户弹框---end -->

<!-- 修改用户弹框---start -->
        <el-dialog title="修改用户" :visible.sync="dialogFormVisible2" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
                
                <el-row :gutter="2">
                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">登录名:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input v-model="UserName02" placeholder="登录名" size="mini"></el-input>
                      </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">密码:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                         <el-form-item label="">
                            <el-input v-model="Password02" placeholder="密码" size="mini"></el-input>
                         </el-form-item>
                    </div>
                    </el-col>
                </el-row>

                <el-row :gutter="2">
                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">真实姓名:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                        <el-input v-model="RealName02" placeholder="真实姓名" size="mini"></el-input>
                       </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">所在科室:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                         <el-form-item label="">
                          <el-select  placeholder="所在科室" 
                           size="mini" v-model="DeptID02" >
                           <el-option
                                  v-for="item in categories3"
                                  :key="item.ID"
                                  :label="item.DeptName"
                                  :value="item.ID">
                            </el-option>
                            </el-select> 
                           </el-form-item>
                    </div>
                    </el-col>
                </el-row>

                <el-row :gutter="2">
                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">用户类别:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-select  placeholder="用户类别" 
                           size="mini" v-model="UseType02" >
                               <el-option label="医院管理员" :value="1"></el-option>
                               <el-option label="挂号收费员" :value="2"></el-option>
                               <el-option label="门诊医生"   :value="3"></el-option>
                               <el-option label="医技医生"   :value="4"></el-option>
                               <el-option label="药房操作员" :value="5"></el-option>
                               <el-option label="财务管理员" :value="6"></el-option>
                        </el-select>
                      </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">医生职称:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                         <el-form-item label="">
                          <el-select  placeholder="医生职称" 
                           size="mini"  v-model="DocTitleID02" >
                             <el-option
                            v-for="item in categories1"
                            :key="item.ID"
                            :label="item.ConstantName"
                            :value="item.ID">
                          </el-option>
                          </el-select>
                         </el-form-item>
                    </div>
                    </el-col>
                </el-row>
                
                <el-row :gutter="2">

                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">挂号级别:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                         <el-form-item label="">
                          <el-select  placeholder="挂号级别" 
                           size="mini"  v-model="RegistLeID02" >
                          <el-option
                            v-for="item in categories4"
                            :key="item.ID"
                            :label="item.RegistName"
                            :value="item.ID">
                          </el-option>
                     </el-select> 
                    </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="5">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">是否参与排班:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                        <el-form-item label="">
                        <el-radio v-model="IsScheduling02" label="是">是</el-radio>
                        <el-radio v-model="IsScheduling02" label="否">否</el-radio>
                        </el-form-item>
                    </div>
                    </el-col>
                </el-row>
            </el-form>
            <br>
            <el-button type="warning" @click="dialogFormVisible2 = false" size="mini">取 消</el-button>
            <el-button type="primary" @click="update" size="mini">保 存</el-button>
            <el-button type="primary" @click="clear" size="mini">清 空</el-button>
           
        </el-dialog>
<!-- 修改用户弹框---end -->
</gao-el-container>
</template>

<script>
  import {postRequest} from '../../utils/api'
  import {putRequest} from '../../utils/api'
  import {deleteRequest} from '../../utils/api'
  import {getRequest} from '../../utils/api'
  export default{
    methods: {
      addNewCate(){
        var _this = this;
        if (this.UserName02 == null || this.UserName02.length == 0
           || this.Password02 == null || this.Password02.length == 0
           || this.RealName02 == null || this.RealName02.length == 0
           || this.UseType02 == null || this.UseType02.length == 0
           || this.DeptID02 == null || this.DeptID02.length == 0

           //|| this.IsScheduling02 == null || this.IsScheduling02.length == 0
           //|| this.RegistLeID02 == null || this.RegistLeID02.length == 0
           //|| this.DocTitleID02 == null || this.DocTitleID02.length == 0 
           
        ) {
            _this.$message({
              type: 'info',
              message: '前5项数据不能为空!'
            });
          } 
          else {
            this.loading = true;
            postRequest('/neusys/user/add', {UserName: this.UserName02, Password: this.Password02, RealName: this.RealName02,UseType: this.UseType02, IsScheduling: this.IsScheduling02,DeptID: this.DeptID02, RegistLeID: this.RegistLeID02, DocTitleID: this.DocTitleID02}).then(resp=> {
            if (resp.status == 200) {
              var json = resp.data;
              _this.$message({type: json.status, message: json.msg});
              
              _this.UserName02 = '';
              _this.Password02 = '';
              _this.RealName02 = '';
              _this.UseType02 = '';
               _this.DeptID02 ='';

              _this.IsSchedulin02 = '';
              _this.RegistLeID02 = '';
              _this.DocTitleID02 ='';
             
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
      }},
      searchClick(){
        this.loadBlogs(1, 100);//临时不翻页的情况
      },
      //翻页相关  
      loadBlogs(page, count){
        var _this = this;
        //02=neusys  01=ucsys
        var url = "/neusys/user/all?page=" + page + "&count=" + count + "&keywords=" + this.keywords;
        getRequest(url).then(resp=> {
         
          if (resp.status == 200) {
            _this.tableData = resp.data.list;
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

        //_this.loading = false;
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
      handleEdit2(index, row){
        var _this = this;

        _this.dialogFormVisible2 = true;
        _this.UserName02 = row.UserName;
        _this.Password02 = row.Password;
        _this.RealName02 = row.RealName;
        _this.UseType02 = row.UseType;
        _this.DeptID02 = row.dpid;

        _this.DocTitleID02 =row.dtid;
        _this.IsScheduling02 = row.IsScheduling;  
        _this.RegistLeID02 = row.reid;
        _this.rowid02=row.ID;


        //_this.UseTypeID02 = row.uid;
        //_this.DocTitleID02 = row.DocTitleID;
        //_this.DeptId02 = row.DeptId;
        //_this.RegistId02 = row.RegistId;
        //console.log("BBBBBB");
      },
      update(){
        var _this = this;
        if (this.UserName02 == null || this.UserName02.length == 0
           || this.Password02 == null || this.Password02.length == 0
           || this.RealName02 == null || this.RealName02.length == 0)
        {
            _this.$message({
              type: 'info',
              message: '前 3项数据不能为空!'
            });
        } 
        else {
        //01=/ucsys  02=/neusys
        putRequest("/neusys/user/update", {ID: this.rowid02, UserName: this.UserName02, 
         Password: this.Password02, RealName: this.RealName02,UseType: this.UseType02, DocTitleID: this.DocTitleID02, IsScheduling: this.IsScheduling02, DeptID: this.DeptID02, RegistLeID: this.RegistLeID02}).then(resp=> {
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
            _this.UserName02 = '';
            _this.Password02 = '';
            _this.RealName02 = '';
            _this.UseType02 = '';
            _this.DocTitleID02 = '';
            _this.IsScheduling02 = '';
            _this.DeptID2 = '';
            _this.RegistLeID02 = '';

            _this.refresh();
            _this.dialogFormVisible2 = false;
      }},
      clear(){
              
              var _this = this;
              _this.UserName02 = '';
              _this.Password02 = '';
              _this.RealName02 = '';
              _this.UseType02 = '';
               _this.DeptID02 ='';
               
              _this.IsSchedulin02 = '';
              _this.RegistLeID02 = '';
              _this.DocTitleID02 ='';

      },

      handleDelete(index, row){
        let _this = this;
        this.$confirm('确认删除 "' + row.RealName + '" ?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          _this.deleteCate(row.ID);
        }).catch(() => {
          //取消
          _this.loading = false;
        });
      },
      newDept(){
        let _this = this;
        this.dialogFormVisible = true;

        _this.UserName02 = '';
        _this.Password02 = '';
        _this.RealName02 = '';
        _this.UseType02 = '';
        _this.IsSchedulin02 = '';
        _this.RegistID02 = '';
        _this.DocTitleID02 ='';
        _this.DeptID02 ='';
      },
      deleteCate(ids){
        var _this = this;
        this.loading = true;
        //删除  01=ucsys  02/neusys
        deleteRequest("/neusys/user/" + ids).then(resp=> {
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
        //01=/ucsys  02=/neusys
        getRequest("/neusys/user/all?count=100").then(resp=> {
          //_this.categories = resp.data;
          _this.tableData = resp.data.list;
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

        //所在科室  01=/ucsys  02=/neusys
        getRequest("/neusys/department/all?count=200").then(resp=> {
            _this.categories3 = resp.data.list;
        }); 
        //挂号级别  
        getRequest("/neusys/registLevel/all?count=200").then(resp=> {
            _this.categories4 = resp.data.list;
        }); 
        //改进后  医生职称
        getRequest("/neusys/constantItem/listByType?keywords=DocTitle").then(resp=> {
            _this.categories1 = resp.data.list;
        });

        
        //用户类别
        //getRequest("/ucsys/constantItem/all2?page=1&count=100&keywords=3").then(resp=> {
        //    _this.categories2 = resp.data.list;
        //});
        //医生职称
        //getRequest("/ucsys/constantItem/all2?page=1&count=100&keywords=9").then(resp=> {
        //    _this.categories1 = resp.data.list;
        //});
       }
    },
    mounted: function () {
      this.loading = true;
      this.refresh();
    },
     filters:{
      showScope:function(scope){
        if(scope=='1'){
          return '医院管理员';
        }else if(scope=='2'){
          return '挂号收费员';
        }else if(scope=='3'){
          return '门诊医生';
        }else if(scope=='4'){
          return '医技医生';
        }
        else if(scope=='5'){
          return '药房操作员';
        }else if(scope=='6'){
          return '财务管理员';
        }else{
          return '其他';
        }
      }
    },
    data(){
      return {
        cityName: '',
        keywords: '',
        selItems: [],
        categories: [],

        categories4: [],
        categories3: [],
        categories2: [],
        categories1: [],
        
        tableData: [],
        loading: false,
        
        DeptID02: '',
        UseType02: '',
        DocTitleID02: '',
        RegistLeID02: '',
        IsScheduling02: '',

        dialogAddVisible: false,
        dialogFormVisible: false,
        dialogFormVisible2: false,
        rowid02: '',
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