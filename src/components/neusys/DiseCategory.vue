<template>
  <gao-el-container>
   <el-row :gutter="2">
      <el-col :span="4">
      <div class="grid-content bg-purple">
           <el-input
        placeholder="请输入疾病分类编码或名称" size="mini"
        v-model="keywords" style="width: 180px;">
      </el-input>
      </div>
      </el-col>

      <el-col :span="4">
      <div class="grid-content bg-purple">
           <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-view" @click="searchClick">查询疾病分类</el-button>
      </div>
      </el-col>

      <el-col :span="4">
      <div class="grid-content bg-purple">
           <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept()">新增疾病分类</el-button>
      </div>
      </el-col>
      <!--
      <el-col :span="4">
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
            
            <el-button size="mini" type="primary" icon="el-icon-sold-out">导入疾病名称</el-button>
            
          </el-upload>
      </div>
      </el-col>
      -->

      <el-col :span="3">
      <div class="grid-content bg-purple">
           <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept2()">疾病管理</el-button>
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
          width="55" align="left">
        </el-table-column>

        <el-table-column
          label="显示顺序号"
          prop="SequenceNo"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="分类编码"
          prop="DicaCode"
          width="120" align="left">
        </el-table-column>
        <el-table-column
          label="分类名称"
          prop="DicaName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="ID"
          prop="ID"
          width="120" 
          align="left" 
          v-if='false'
          >
        </el-table-column>
       
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
       <div>
      <el-row :gutter="2">
            <el-col :span="3">
            <el-button type="danger" :disabled="this.selItems.length==0" style="margin-top: 10px;width: 100px;" size="mini"
                 @click="deleteAll" v-if="this.tableData2.length>0">批量删除
            </el-button>
            </el-col>
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

<!-- 新增疾病分类弹框---start -->
        <el-dialog title="新增疾病分类" :visible.sync="dialogFormVisible" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
               
                <el-row :gutter="2">
                    <el-col :span="5">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">疾病分类编码:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                           <el-input v-model="DicaCode02" placeholder="疾病分类编码" size="mini"></el-input>
                       </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="5">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">疾病分类名称:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                        <el-form-item label="">
                            <el-input v-model="DicaName02" placeholder="疾病分类名称" size="mini"></el-input>
                        </el-form-item>
                    </div>
                    </el-col>
                </el-row>

                <el-row :gutter="2">
                    <el-col :span="5">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">显示顺序号:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input v-model="SequenceNo02" placeholder="显示顺序号" size="mini"></el-input>
                       </el-form-item>
                    </div>
                    </el-col>
                 </el-row>
             </el-form>
            <br>
            <el-button type="warning" @click="dialogFormVisible = false" size="mini">取 消</el-button>
            <el-button type="primary" @click="addNewCate" size="mini">保 存</el-button>
        </el-dialog>
<!-- 新增疾病分类弹框---end -->

<!-- 修改疾病分类弹框---start -->
        <el-dialog title="修改疾病分类" :visible.sync="dialogFormVisible22" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
               
                <el-row :gutter="2">
                    <el-col :span="5">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">疾病分类编码:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                           <el-input v-model="DicaCode02" placeholder="疾病分类编码" size="mini"></el-input>
                       </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="5">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">疾病分类名称:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                        <el-form-item label="">
                            <el-input v-model="DicaName02" placeholder="疾病分类名称" size="mini"></el-input>
                        </el-form-item>
                    </div>
                    </el-col>
                </el-row>

                <el-row :gutter="2">
                    <el-col :span="5">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">显示顺序号:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                          <el-input v-model="SequenceNo02" placeholder="显示顺序号" size="mini"></el-input>
                       </el-form-item>
                    </div>
                    </el-col>
                 </el-row>
             </el-form>
            <br>
            <el-button type="warning" @click="dialogFormVisible22 = false" size="mini">取 消</el-button>
            <el-button type="primary" @click="update22" size="mini">保 存</el-button>
        </el-dialog>
<!-- 修改疾病分类弹框---end -->

<!-- 疾病管理 弹框---start -->
   <el-dialog title="疾病列表" :visible.sync="dialogAddVisible" width="750px" align="center">

   <el-input
        placeholder="请输入疾病编码或名称" size="mini"
        v-model="keywords202" style="width: 200px;">
   </el-input>
    
   <el-button type="primary" size="mini" style="margin-left: 10px" @click="searchClick202">查询疾病</el-button>

   <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept3()">新增疾病</el-button>
 
   <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formAdd" class="demo-form-inline">
        
      <el-table
        ref="multipleTable"
        :data="tableData"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange202" v-loading="loading">

        <el-table-column
          type="selection"
          width="55" align="left">
        </el-table-column>
        
        <el-table-column
          label="疾病编码"
          prop="DiseaseCode"
          width="120" align="left">
        </el-table-column>
        <el-table-column
          label="疾病名称"
          prop="DiseaseName"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="国际ICD编码"
          prop="DiseaseICD"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="疾病所属分类"
          prop="DicaName"
          width="120" align="left">
        </el-table-column>

         <el-table-column
          label="疾病所属分类id"
          prop="DiseCategoryID"
          width="120" align="left" v-if='false'>
        </el-table-column>

        <el-table-column
          label="ID"
          prop="ID"
          width="120" 
          align="left" 
          v-if='false'
          >
        </el-table-column>

         <el-table-column label="操作" align="left">
          <template slot-scope="scope">
            <el-button type="primary"
              size="mini"
              @click="handleEdit33(scope.$index, scope.row)">编辑
            </el-button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete02(scope.$index, scope.row)">删除
            </el-button>
          </template>
        </el-table-column>
        </el-table>
      </el-form>

      <el-pagination
              background
              :page-size="pageSize202" :current-page.sync="currentPage202"
              layout="prev, pager, next"
              :total="totalCount202" @current-change="currentChange202" v-show="this.tableData.length>0">
      </el-pagination>

      <br>
      <el-button type="danger" @click="deleteAll202" size="mini" :disabled="this.selItems202.length==0">批量删除</el-button>
      
      <el-button type="primary" @click="dialogAddVisible = false" size="mini">关闭</el-button>
</el-dialog>
<!-- 疾病管理  弹框---end -->


<!-- 新增疾病 弹框---start -->
<el-dialog title="新增疾病" :visible.sync="dialogFormVisible2" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
                
  <el-row :gutter="2">
      
      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">疾病编码:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
          <el-form-item label="">
                    <el-input v-model="DiseaseCode02" placeholder="疾病编码" size="mini"></el-input>
          </el-form-item>
      </div>
      </el-col>

      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">疾病名称:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
          <el-form-item label="">
                    <el-input v-model="DiseaseName02" placeholder="疾病名称" size="mini"></el-input>
          </el-form-item>
      </div>
      </el-col>
  </el-row>

  <el-row :gutter="2">
      
      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">国际ICD编码:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
          <el-form-item label="">
                    <el-input v-model="DiseaseICD02" placeholder="国际ICD编码" size="mini"></el-input>
          </el-form-item>
      </div>
      </el-col>

      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">疾病所属分类:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
           <el-form-item label="">
               <el-select  placeholder="疾病所属分类" 
                     size="mini" v-model="DiseCategoryID02">
                     <el-option
                            v-for="item in categories3"
                            :key="item.ID"
                            :label="item.DicaName"
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
<el-button type="primary" @click="addNewCate02" size="mini">保 存</el-button>
</el-dialog>
<!-- 新增疾病 弹框---end  -->

<!-- 修改疾病弹框---start -->
<el-dialog title="修改疾病" :visible.sync="dialogFormVisible33" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
                
  <el-row :gutter="2">
      
      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">疾病编码:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
          <el-form-item label="">
                    <el-input v-model="DiseaseCode02" placeholder="疾病编码" size="mini"></el-input>
          </el-form-item>
      </div>
      </el-col>

      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">疾病名称:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
          <el-form-item label="">
                    <el-input v-model="DiseaseName02" placeholder="疾病名称" size="mini"></el-input>
          </el-form-item>
      </div>
      </el-col>
  </el-row>

  <el-row :gutter="2">
      
      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">国际ICD编码:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
          <el-form-item label="">
                    <el-input v-model="DiseaseICD02" placeholder="国际ICD编码" size="mini"></el-input>
          </el-form-item>
      </div>
      </el-col>

      <el-col :span="5">
      <div class="grid-content bg-purple">
          <font style="font-size:13px;font-family:'Microsoft YaHei'">疾病所属分类:</font>&nbsp;
      </div>
      </el-col>

      <el-col :span="7">
      <div class="grid-content bg-purple">
           <el-form-item label="">
               <el-select  placeholder="疾病所属分类" 
                     size="mini" v-model="DiseCategoryID02">
                     <el-option
                            v-for="item in categories3"
                            :key="item.ID"
                            :label="item.DicaName"
                            :value="item.ID">
                      </el-option>
                     
                </el-select>
            </el-form-item>
      </div>
      </el-col>
  </el-row>
</el-form>
<br>
<el-button type="warning" @click="dialogFormVisible33 = false" size="mini">取 消</el-button>
<el-button type="primary" @click="update33" size="mini">保 存</el-button>
</el-dialog>
<!-- 修改疾病弹框---end  -->

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
        if (this.DicaCode02 == null || this.DicaCode02.length == 0
           ||this.DicaName02 == null || this.DicaName02.length == 0
           ||this.SequenceNo02 == null || this.SequenceNo02.length == 0
        ) {
            _this.$message({
              type: 'info',
              message: '3数据不能为空!'
            });
          } 
          else {
            this.loading = true;
            //ucsys neusys
            postRequest('/neusys/diseCategory/add', {DicaCode: this.DicaCode02, DicaName: this.DicaName02, SequenceNo: this.SequenceNo02}).then(resp=> {
            if (resp.status == 200) {
              var json = resp.data;
              _this.$message({type: json.status, message: json.msg});
              _this.DicaCode02 = '';
              _this.DicaName02 = '';
              _this.SequenceNo02 = '';
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
      addNewCate02(){
        var _this = this;
        if (this.DiseaseCode02 == null || this.DiseaseCode02.length == 0
           ||this.DiseaseName02 == null || this.DiseaseName02.length == 0
           ||this.DiseaseICD02 == null || this.DiseaseICD02.length == 0
           ||this.DiseCategoryID02 == null || this.DiseCategoryID02.length == 0
        ) {
            _this.$message({
              type: 'info',
              message: '4 项数据不能为空!'
            });
          } 
          else {
            this.loading = true;
            //ucsys neusys
            postRequest('/neusys/disease/add', {DiseaseCode: this.DiseaseCode02, DiseaseName: this.DiseaseName02, DiseaseICD: this.DiseaseICD02, DiseCategoryID: this.DiseCategoryID02}).then(resp=> {
            if (resp.status == 200) {
              var json = resp.data;
              _this.$message({type: json.status, message: json.msg});
              _this.DiseaseCode02 = '';
              _this.DiseaseName02 = '';
              _this.DiseaseICD02 = '';
              _this.DiseCategoryID02 = '';
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
      //翻页相关 
      searchClick(){
         this.loadBlogs(1, this.pageSize);
      },
      //翻页相关  
      loadBlogs(page, count){
        var _this = this;
        var url = "/neusys/diseCategory/all?page=" + page + "&count=" + count + "&keywords=" + this.keywords;
        getRequest(url).then(resp=> {
        if (resp.status == 200) {
            _this.loading = false;
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
          //压根没见到服务器
          _this.loading = false;
          _this.$message({type: 'error', message: '数据加载失败!'});
        })
        //_this.loading = false;
      },
      searchClick202(){
        //this.loadBlogs202(1, 10);//临时不翻页
        this.loadBlogs202(1, this.pageSize202);
      },
      loadBlogs202(page, count){
        var _this = this;
        //neusys  ucsys
        var url = "/neusys/disease/all?page=" + page + "&count=" + count + "&keywords=" + this.keywords202;
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
          //压根没见到服务器
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

        _this.dialogFormVisible22 = true;
        _this.DicaCode02 = row.DicaCode;
        _this.DicaName02 = row.DicaName;
        _this.SequenceNo02 = row.SequenceNo;
       _this.rowid02=row.ID;
        //console.log("BBBBBB");
      },
      update22(){
        var _this = this;
        if (this.DicaCode02 == null || this.DicaCode02.length == 0
           || this.DicaName02 == null || this.DicaName02.length == 0
           || this.SequenceNo02 == null || this.SequenceNo02.length == 0 
           )
        {
            _this.$message({
              type: 'info',
              message: '3 项数据不能为空!'
            });
        } 
        else {
        //neusys   ucsys
        putRequest("/neusys/diseCategory/update", {id: this.rowid02, DicaCode: this.DicaCode02, 
         DicaName: this.DicaName02, SequenceNo: this.SequenceNo02}).then(resp=> {
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
            _this.DicaCode02 = '';
            _this.DicaName02 = '';
            _this.SequenceNo02 = '';
            _this.refresh();
            _this.dialogFormVisible22 = false;
        }},
        handleEdit33(index, row){
        var _this = this;

        _this.dialogFormVisible33 = true;
        _this.DiseaseCode02 = row.DiseaseCode;
        _this.DiseaseName02 = row.DiseaseName;
        _this.DiseaseICD02 = row.DiseaseICD;
        _this.DicaId02 = row.DicaID;

        _this.DiseCategoryID02 = row.DiseCategoryID;

       _this.rowid33=row.ID;
        //console.log("BBBBBB");
      },
      update33(){
        var _this = this;
        if (this.DiseaseCode02 == null || this.DiseaseCode02.length == 0
           || this.DiseaseName02 == null || this.DiseaseName02.length == 0
           || this.DiseaseICD02 == null || this.DiseaseICD02.length == 0
           || this.DiseCategoryID02 == null || this.DiseCategoryID02.length == 0
           )
        {
            _this.$message({
              type: 'info',
              message: '4 项数据不能为空!'
            });
        } 
        else { 
        //neusys  ucsys  
        putRequest("/neusys/disease/update", {ID: this.rowid33, DiseaseCode: this.DiseaseCode02, 
         DiseaseName: this.DiseaseName02, DiseaseICD: this.DiseaseICD02, DiseCategoryID: this.DiseCategoryID02}).then(resp=> {
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
            _this.DiseaseCode02 = '';
            _this.ExpName02 = '';
            _this.DiseaseICD02 = '';
            _this.DicaId02 = '';
            _this.refresh();
            _this.dialogFormVisible33 = false;
            
       }},
      handleDelete(index, row){
        let _this = this;
        this.$confirm('确认删除 "' + row.DicaName + '" ?', '提示', {
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
      handleDelete02(index, row){
        let _this = this;
        this.$confirm('确认删除 "' + row.DiseaseName + '" ?', '提示', {
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
        _this.DicaCode02 = "";
        _this.DicaName02 = "";
        _this.SequenceNo02 = "";
        _this.dialogFormVisible = true;
      },
      newDept2(){
        let _this = this;
        _this.dialogAddVisible = true;
        tableData="";
      },
      newDept3(){
        let _this = this;
        //DiseaseCode02
        _this.DiseaseCode02 = "";
        _this.DiseaseName02 = "";
        _this.DiseaseICD02 = "";
        _this.DicaId02 = "";
        _this.dialogFormVisible2 = true;
      },
      deleteCate(ids){
        var _this = this;
        this.loading = true;
        //删除  neusys  ucsys
        deleteRequest("/neusys/diseCategory/" + ids).then(resp=> {
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
        //删除  neusys  ucsys
        deleteRequest("/neusys/disease/" + ids).then(resp=> {
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

        //疾病分类  
        getRequest("/neusys/diseCategory/all?count=500").then(resp=> {
            _this.categories3 = resp.data.list;
        }); 

        _this.searchClick();
        _this.loading = false;

       }
    },
    mounted: function () {
      this.loading = true;
      this.refresh();
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

        keywords202:'',
        DiseCategoryID02: '',
        selItems: [],
        categories3: [],
        categories: [],
        categories202: [],
        selItems202: [],
        tableData2: [],
        tableData: [],
        loading: false,
        dialogAddVisible: false,
        dialogFormVisible: false,
        dialogFormVisible2: false,
        dialogFormVisible22: false,
        dialogFormVisible33: false,
        rowid02: '',
        rowid33: '',
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