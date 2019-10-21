<template>
  <gao-el-container>
   <el-row :gutter="2">
      <el-col :span="4">
      <div class="grid-content bg-purple">
           <el-input
        placeholder="请输入常数类别编码或名称" size="mini"
        v-model="keywords" style="width: 180px;">
      </el-input>
      </div>
      </el-col>

      <el-col :span="4">
      <div class="grid-content bg-purple">
           <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-view" @click="searchClick">查询常数类别</el-button>
      </div>
      </el-col>

      <el-col :span="4">
      <div class="grid-content bg-purple">
           <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept()">新增常数类别</el-button>
      </div>
      </el-col>

      <el-col :span="3">
      <div class="grid-content bg-purple">
           <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept2()">常数项管理</el-button>
      </div>
      </el-col>
   </el-row>
   <p></p>
   <!-- :data="categories"  --> 
    <gao-el-main class="cate_mana_main">
      <el-table
        ref="multipleTable"
        :data="tableData2"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange" v-loading="loading">
        <el-table-column
          type="selection"
          width="55" align="left" v-if='false'>
        </el-table-column>
        <el-table-column
          label="类别编码"
          prop="ConstantTypeCode"
          width="120" align="left">
        </el-table-column>
        <el-table-column
          label="类别名称"
          prop="ConstantTypeName"
          width="120" align="left">
        </el-table-column>
        
        <!-- v-if='false' -->
        <el-table-column
          
          label="ID"
          prop="ID"
          width="120" align="left">
        </el-table-column>
       <!--
        <el-table-column label="操作" align="left">
          <template slot-scope="scope">
            
            <el-button type="primary"
              size="mini"
              @click="handleEdit(scope.$index, scope.row)">编辑0
            </el-button>
            
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
        -->
      </el-table>
    <div>
      <el-row :gutter="2">
            <!--
            <el-col :span="12">
            <el-button type="danger" :disabled="this.selItems.length==0" style="margin-top: 10px;width: 100px;" size="mini"
                 @click="deleteAll" v-if="this.tableData2.length>0">批量删除
            </el-button>
            </el-col>
            -->
            <el-col :span="5">
            <el-pagination
              background
              :page-size="pageSize" :current-page.sync="currentPage01"
              layout="prev, pager, next"
              :total="totalCount" @current-change="currentChange" v-show="this.tableData2.length>0">
            </el-pagination>
            </el-col>
      </el-row>
    </div>
</gao-el-main>

<!-- 新增常数类别弹框---start -->
        <el-dialog title="新增常数类别" :visible.sync="dialogFormVisible" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
               
                <el-row :gutter="2">
                    <el-col :span="5">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">常数类别编码:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                           <el-input v-model="ConstantTypeCode3" placeholder="常数类别编码" size="mini"></el-input>
                       </el-form-item>
                    </div>
                    </el-col>
               
                    <el-col :span="5">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">常数类别名称:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input v-model="ConstantTypeName3" placeholder="常数类别名称" size="mini"></el-input>
                       </el-form-item>
                    </div>
                    </el-col>
                 </el-row>
             </el-form>
            <br>
            <el-button type="warning" @click="dialogFormVisible = false" size="mini">取 消</el-button>
            <el-button type="primary" @click="addNewCate" size="mini">保 存</el-button>
        </el-dialog>
<!-- 新增常数类别弹框---end -->

<!-- 修改常数类型弹框---start -->
    <el-dialog title="修改常数类型" :visible.sync="dialogFormVisible3" width="550px" align="center">
      
      <font style="font-size:13px;font-family:'Microsoft YaHei'">常数项编码:</font>&nbsp;
      <el-input
        placeholder="常数类型编码"
        v-model="ConstantTypeCode2" style="width: 200px;" size="mini">
      </el-input>
      <br><br>
      <font style="font-size:13px;font-family:'Microsoft YaHei'">常数项名称:</font>&nbsp;
      <el-input
        placeholder="常数类型名称"
        v-model="ConstantTypeName2" style="width: 200px;" size="mini">
      </el-input>

      <br><br>
          <el-button type="warning" @click="dialogFormVisible3 = false" size="mini">取 消</el-button>
          <el-button type="primary" size="mini" style="margin-left: 10px" @click="updaeConstantType">保存</el-button>
      
</el-dialog>
<!-- 修改常数类型弹框---end -->


<!-- 常数项管理 弹框---start -->
   <el-dialog title="常数项列表" :visible.sync="dialogAddVisible" width="850px" align="center">

   <el-input
        placeholder="请输入常数项编码或名称" size="mini"
        v-model="keywords202" style="width: 200px;">
   </el-input>

    <el-select  placeholder="所属分类"  v-model="keywords203"  size="mini" >
        <el-option
              v-for="item in categories3"
              :key="item.ID"
              :label="item.ConstantTypeName"
              :value="item.ID">
        </el-option>
    </el-select>
    
   <el-button type="primary" size="mini" style="margin-left: 10px" @click="searchClick202">查询常数项</el-button>

   <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept3()">新增常数项</el-button>

   <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formAdd" class="demo-form-inline">
        
      <el-table
        ref="multipleTable"
        :data="tableData"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange202" v-loading="loading">

        <!--
        <el-table-column
          type="selection"
          width="55" align="left">
        </el-table-column>
        -->

        <el-table-column
          label="常数项编码"
          prop="ConstantCode"
          width="120" align="left">
        </el-table-column>
        <el-table-column
          label="常数项名称"
          prop="ConstantName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="常数项类别"
          prop="ConstantTypeName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="常数项类别id"
          prop="ConstantTypeID"
          width="120" align="left"
          v-if='false'
          >
        </el-table-column>

        <el-table-column
          label="常数项ID"
          prop="ID"
          width="120" align="left" >
        </el-table-column>

        <!--
        <el-table-column label="操作" align="left">
          <template slot-scope="scope">
            <el-button type="primary"
              size="mini"
              @click="handleEdit32(scope.$index, scope.row)">编辑
            </el-button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete202(scope.$index, scope.row)">删除
            </el-button>
          </template>
        </el-table-column>
        -->

        </el-table>
      </el-form>
      
      <el-pagination
              background
              :page-size="pageSize202" :current-page.sync="currentPage202"
              layout="prev, pager, next"
              :total="totalCount202" @current-change="currentChange202" v-show="this.tableData.length>0">
      </el-pagination>

      <br>
      <!--
      <el-button type="danger" @click="deleteAll202" size="mini" :disabled="this.selItems202.length==0">批量删除</el-button>
      -->
      <el-button type="primary" @click="dialogAddVisible = false" size="mini">关闭</el-button>
            
</el-dialog>
<!-- 常数项管理弹框---end -->


<!-- 新增常数项弹框---start -->
<el-dialog title="新增常数项" :visible.sync="dialogFormVisible2" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">

 <el-row :gutter="2">
      
      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">常数项编码:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
          <el-form-item label="">
                    <el-input v-model="ConstantCode4" placeholder="常数项编码" size="mini"></el-input>
          </el-form-item>
      </div>
      </el-col>

      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">常数项名称:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
          <el-form-item label="">
                    <el-input v-model="ConstantName4" placeholder="常数项名称" size="mini"></el-input>
          </el-form-item>
      </div>
      </el-col>
  </el-row>

  <el-row :gutter="2">
      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">所属分类:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
      
      <el-form-item label="">
          <el-select  placeholder="所属分类"  v-model="ConstantTypeID4"  size="mini" >
              <el-option
                   v-for="item in categories3"
                   :key="item.ID"
                   :label="item.ConstantTypeName"
                   :value="item.ID">
              </el-option>
          </el-select>
      </el-form-item>
      </div>
      </el-col>
  </el-row>
</el-form>
<br>
<el-button type="warning" @click="dialogFormVisible2 = false" size="mini">取 消</el-button>
<el-button type="primary" @click="addNewCate202()" size="mini">保 存</el-button>
</el-dialog>
<!-- 新增常数项弹框---end  -->

<!-- 修改常数项弹框---start -->
<el-dialog title="修改常数项" :visible.sync="dialogFormVisible32" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">

 <el-row :gutter="2">
      
      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">常数项编码:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
          <el-form-item label="">
                    <el-input v-model="ConstantCode4" placeholder="常数项编码" size="mini"></el-input>
          </el-form-item>
      </div>
      </el-col>

      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">常数项名称:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
          <el-form-item label="">
                    <el-input v-model="ConstantName4" placeholder="常数项名称" size="mini"></el-input>
          </el-form-item>
      </div>
      </el-col>
  </el-row>

  <el-row :gutter="2">
      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">所属分类:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
      
      <el-form-item label="">
          <el-select  placeholder="所属分类"  v-model="ConstantTypeID4"  size="mini" >
              <el-option
                   v-for="item in categories3"
                   :key="item.ID"
                   :label="item.ConstantTypeName"
                   :value="item.ID">
              </el-option>
          </el-select>
      </el-form-item>
      </div>
      </el-col>
  </el-row>
</el-form>
<br>
<el-button type="warning" @click="dialogFormVisible32 = false" size="mini">取 消</el-button>
<el-button type="primary" @click="updaeConstant()" size="mini">保 存</el-button>
</el-dialog>
<!-- 修改常数项弹框---end -->

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
        if (this.ConstantTypeCode3 == null || this.ConstantTypeCode3.length == 0
            || this.ConstantTypeName3 == null || this.ConstantTypeName3.length == 0) {
            _this.$message({
              type: 'info',
              message: '2项数据不能为空!'
            });
          } 
          else {
            this.loading = true;
            //01=/ucsys/constantType/add   02=neusys
            postRequest('/neusys/constantType/add', {ConstantTypeCode: this.ConstantTypeCode3, ConstantTypeName: this.ConstantTypeName3}).then(resp=> {
            if (resp.status == 200) {
              var json = resp.data;
              _this.$message({type: json.status, message: json.msg});
              _this.ConstantTypeCode3 = '';
              _this.ConstantTypeName3 = '';
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
      
      addNewCate202(){
        var _this = this;
        if (this.ConstantCode4 == null || this.ConstantCode4.length == 0
            || this.ConstantName4 == null || this.ConstantName4.length == 0) {
            _this.$message({
              type: 'info',
              message: '2项数据不能为空!'
            });
          } 
          else {
            this.loading = true;
            //01=/ucsys/constantItem/add   02=/neusys
            postRequest('/neusys/constantItem/add', {ConstantCode: this.ConstantCode4, ConstantName: this.ConstantName4, ConstantTypeID: this.ConstantTypeID4}).then(resp=> {
            if (resp.status == 200) {
              var json = resp.data;
              _this.$message({type: json.status, message: json.msg});
              _this.ConstantCode4 = '';
              _this.ConstantName4 = '';
              _this.ConstantTypeID4= '';
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
        }).catch(() => {
          //取消
          _this.loading = false;
        });
      },
      deleteAll202(){
        var _this = this;
        this.$confirm('确认删除这 ' + this.selItems202.length + ' 条数据?', '提示', {
          type: 'warning',
          confirmButtonText: '确定',
          cancelButtonText: '取消'
        }).then(()=> {
          var selItems202 = _this.selItems202;
          var ids = '';
          for (var i = 0; i < selItems202.length; i++) {
            ids += selItems202[i].ID + ",";
          }
          _this.deleteCate202(ids.substring(0, ids.length - 1));
        }).catch(() => {
          //取消
          _this.loading = false;
        });
      },
      handleSelectionChange(val) {
        this.selItems = val;
      },
      handleSelectionChange202(val) {
        this.selItems202 = val;
      },
      handleEdit2(index, row){
        var _this = this;
        _this.dialogFormVisible3 = true;
        _this.ConstantTypeCode2=row.ConstantTypeCode;
        _this.ConstantTypeName2=row.ConstantTypeName;
        _this.rowid2=row.ID;
        //console.log("浏览器控制台输出：");
       },
       handleEdit32(index, row){
        var _this = this; 
        _this.dialogFormVisible32 = true;
        _this.ConstantCode4=row.ConstantCode;
        _this.ConstantName4=row.ConstantName;
        _this.ConstantTypeID4=row.ConstantTypeID;
        _this.rowid4=row.ID;
        //console.log("浏览器控制台输出：");
      },

      updaeConstantType(){
        var _this = this;
        if (this.ConstantTypeCode2 == null || this.ConstantTypeCode2.length == 0
           || this.ConstantTypeName2 == null || this.ConstantTypeName2.length == 0)
        {
            _this.$message({
              type: 'info',
              message: '2项数据不能为空!'
            });
        } 
        else {
            //01=/ucsys/constantType/update   02=/neusys/constantType/update
            putRequest("/neusys/constantType/update", {id: this.rowid2, ConstantTypeCode: this.ConstantTypeCode2,  ConstantTypeName: this.ConstantTypeName2}).then(resp=> {
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
                _this.ConstantTypeCode2 = '';
                _this.ConstantTypeName2 = '';
                _this.dialogFormVisible3 = false
                _this.refresh();
       }},

       updaeConstant(){
        var _this = this;
        if (this.ConstantCode4 == null || this.ConstantCode4.length == 0
           || this.ConstantName4 == null || this.ConstantName4.length == 0)
        {
            _this.$message({
              type: 'info',
              message: '2项数据不能为空!'
            });
        } 
        else {
        //01=/ucsys/constantItem/update   02=/neusys
        putRequest("/neusys/constantItem/update", {id: this.rowid4, ConstantCode: this.ConstantCode4, 
         ConstantName: this.ConstantName4, ConstantTypeID: this.ConstantTypeID4}).then(resp=> {
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
            _this.ConstantCode4 = '';
            _this.ConstantName4 = '';
            _this.ConstantTypeID4= '';
            _this.dialogFormVisible32 = false
            _this.refresh();
       }},

      handleDelete(index, row){
        let _this = this;
        this.$confirm('确认删除 "' + row.ConstantTypeName + '" ?', '提示', {
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
      handleDelete202(index, row){
        let _this = this;
        this.$confirm('确认删除 "' + row.ConstantName + '" ?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          _this.deleteCate202(row.ID);
        }).catch(() => {
          //取消
          _this.loading = false;
        });
      },
      newDept(){
        let _this = this;
        this.dialogFormVisible = true;
      },
      newDept2(){
        let _this = this;
        this.dialogAddVisible = true;
        _this.keywords202="";
        _this.keywords203="";
        _this.tableData="";
      },
      searchClick202(){
        this.loadBlogs202(1, this.pageSize202);
      },
      //翻页相关 
      searchClick(){
        this.loadBlogs(1, this.pageSize);
      },
      //翻页相关  
      loadBlogs(page, count){
        var _this = this;
        
        var url = "/neusys/constantType/all?page=" + page + "&count=" + count + "&keywords=" + this.keywords + "&keywords2=" + this.keywords2;
        getRequest(url).then(resp=> {
          _this.loading = false;
          if (resp.status == 200) {
            _this.tableData2 = resp.data.list;
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
          //没服务器
          _this.loading = false;
          _this.$message({type: 'error', message: '数据加载失败!'});
        })
      },
       loadBlogs202(page, count){
        var _this = this;
        //01=/ucsys/constantItem/all  02=neusys
        var url = "/neusys/constantItem/all?page=" + page + "&count=" + count + "&keywords=" + this.keywords202 + "&keywords2=" + this.keywords203;
        getRequest(url).then(resp=> {
          _this.loading = false;
          if (resp.status == 200) {
            _this.tableData = resp.data.list;
            _this.totalCount202 = resp.data.totalCount;
            _this.currentPage202 = page ;
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
          //没服务器
          _this.loading = false;
          _this.$message({type: 'error', message: '数据加载失败!'});
        })
      },
      //翻页相关
      currentChange(currentPage){
        this.currentPage01 = currentPage;
        this.loading = true;
        this.loadBlogs(currentPage, this.pageSize);
      },
      //翻页相关
      currentChange202(currentPage202){
        this.currentPage202 = currentPage202;
        this.loading = true;
        this.loadBlogs202(currentPage202, this.pageSize202);
      },
      newDept3(){
        let _this = this;
        this.dialogFormVisible2 = true;
      },
      newDept4(){
        let _this = this;
        this.dialogFormVisible5 = true;
      },
      deleteCate(ids){
        var _this = this;
        this.loading = true;
        //删除
        //01=/ucsys/constantType/   02=/neusys/constantType/
        deleteRequest("/neusys/constantType/" + ids).then(resp=> {
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
      deleteCate202(ids){
        var _this = this;
        this.loading = true;
        //删除
        //01=  02=neusys
        deleteRequest("/neusys/constantItem/" + ids).then(resp=> {
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
        _this.tableData2 ="";
        _this.tableData ="";
        //01=ucsys   02=neusys
        getRequest("/neusys/constantType/all").then(resp=> {
          _this.tableData2 = resp.data.list;
          _this.loading = false;
          _this.totalCount = resp.data.totalCount;
          _this.currentPage01 = page;
        }, resp=> {
          if (resp.response.status == 403) {
            _this.$message({
              type: 'error',
              message: resp.response.data
            });
          }
          _this.loading = false;
        });
        //02=neusys   01=ucsys
        getRequest("/neusys/constantItem/all").then(resp=> {
          _this.tableData = resp.data.list;
          _this.loading = false;
          _this.totalCount202 = resp.data.totalCount;
          _this.currentPage202 = page;
        }, resp=> {
          if (resp.response.status == 403) {
            _this.$message({
              type: 'error',
              message: resp.response.data
            });
          }
          _this.loading = false;
        });
        //02=neusys   01=ucsys
        getRequest("/neusys/constantType/all2").then(resp=> {
            _this.categories3 = resp.data.list;
        });
      }
    },
   
    mounted: function () {

      this.loading = true;
      this.refresh();

      //翻页相关 { 
      var _this = this;
      this.loading = true;
      this.loadBlogs(1, this.pageSize);
      
      window.bus.$on('blogTableReload', function () {
        _this.loading = true;
        _this.loadBlogs(_this.currentPage01, _this.pageSize);
      })
      //翻页相关 }
    },
    data(){
      return {
        //翻页相关 { 
        totalCount: 2,
        currentPage01:1,
        pageSize: 10,
        keywords: '',
        keywords2: '',

        totalCount202: 2,
        currentPage202:1,
        pageSize202: 10,
        keywords202: '',
        keywords203: '',
        ConstantTypeID4: '', //有用
        //翻页相关 }

        
        selItems: [],
        tableData:[],
        tableData2:[],
        categories: [],
        categories3: [],
        categories2: [],
        categories202: [],
        selItems202: [],
        loading: false,
        
        dialogAddVisible: false,
        dialogFormVisible: false,
        dialogFormVisible2: false,
        dialogFormVisible3: false,
        dialogFormVisible5: false,
        dialogFormVisible32: false,
        rowid2:'',
        rowid4:'',
        ConstantTypeCode2:'',
        ConstantTypeName2:'',
        ConstantTypeID001:'',
        
        /* 静态数据
          tableData: [{
          id: 'JGJJH',
          name: '肩关节结核',
          province: 'A18.008+',
          city: '关节疾病'},{
          id: 'WGJJH',
          name: '腕关节结核',
          province: 'A18.009+',
          city: '关节疾病'},
        ], 
         tableData2: [{
          ConstantTypeCode: 'jbfl01',
          ConstantTypeName: '细菌性疾病',
          },{
          ConstantTypeCode: 'jbfl02',
          ConstantTypeName: '关节疾病'},
        ]  
        */
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
