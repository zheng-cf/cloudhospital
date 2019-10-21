<template>
  <gao-el-container>
      <el-input
        placeholder="请输入费用科目编码或名称" size="mini"
        v-model="keywords" style="width: 200px;">
      </el-input>
      <el-button type="primary" size="mini" style="margin-left: 10px" @click="searchClick">查询费用科目</el-button>

      <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept()">新增费用科目</el-button>
    
    <!-- :data="categories"  --> 
    <el-main class="gao_cate_mana_main">
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
        <el-table-column
          label="科目编码"
          prop="ExpCode"
          width="120" align="left">
        </el-table-column>
        <el-table-column
          label="科目名称"
          prop="ExpName"
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
            <!--
            <el-button
              size="mini"
              @click="handleEdit(scope.$index, scope.row)">编辑
            </el-button>
            -->  
            <el-button
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
            <el-col :span="12">
            <el-button type="danger" :disabled="this.selItems.length==0" style="margin-top: 10px;width: 100px;" size="mini"
                 @click="deleteAll" v-if="this.tableData.length>0">批量删除
            </el-button>
            </el-col>
            <el-col :span="5">
            <el-pagination
              background
              :page-size="pageSize" :current-page.sync="currentPage01"
              layout="prev, pager, next"
              :total="totalCount" @current-change="currentChange" v-show="this.tableData.length>0">
            </el-pagination>
            </el-col>
      </el-row>
      </div>
    </el-main>
  
<!-- 新增费用科目弹框---start -->
        <el-dialog title="新增费用科目" :visible.sync="dialogFormVisible" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
                <el-form-item label="科目编码">
                    <el-input v-model="ExpCode02" placeholder="科目编码" size="mini"></el-input>
                </el-form-item>
                <el-form-item label="科目名称">
                    <el-input v-model="ExpName02" placeholder="科目名称" size="mini"></el-input>
                </el-form-item>
            </el-form>
           
            <el-button type="warning" @click="dialogFormVisible = false" size="mini">取 消</el-button>
            <el-button type="primary" @click="addNewCate()" size="mini">保 存</el-button>
           
        </el-dialog>
  <!-- 新增费用科目弹框---end -->

  <!-- 修改费用科目弹框---start -->
        <el-dialog title="修改费用科目" :visible.sync="dialogFormVisible2" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
                <el-form-item label="科目编码">
                    <el-input v-model="ExpCode02" placeholder="科目编码" size="mini"></el-input>
                </el-form-item>
                <el-form-item label="科目名称">
                    <el-input v-model="ExpName02" placeholder="科目名称" size="mini"></el-input>
                </el-form-item>
            </el-form>
           
            <el-button type="warning" @click="dialogFormVisible2 = false" size="mini">取 消</el-button>
            <el-button type="primary" @click="update()" size="mini">保 存</el-button>
           
        </el-dialog>
  <!-- 修改费用科目弹框---end -->

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
        if (this.ExpCode02 == null || this.ExpCode02.length == 0
           ||this.ExpName02 == null || this.ExpName02.length == 0
        ) {
            _this.$message({
              type: 'info',
              message: '2数据不能为空!'
            });
          } 
          else {
            this.loading = true;
            postRequest('/neusys/expenseClass/add', {ExpCode: this.ExpCode02, ExpName: this.ExpName02}).then(resp=> {
            if (resp.status == 200) {
              var json = resp.data;
              _this.$message({type: json.status, message: json.msg});
              _this.ExpCode02 = '';
              _this.ExpName02 = '';
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
      //翻页相关 
      searchClick(){
         //this.loadBlogs(1, 100);//this.pageSize  100   临时不翻页
         this.loadBlogs(1, this.pageSize);
      },
      //翻页相关  
      loadBlogs(page, count){
        var _this = this;
        var url = "/neusys/expenseClass/all?page=" + page + "&count=" + count + "&keywords=" + this.keywords;
        getRequest(url).then(resp=> {
            _this.loading = false;
          //_this.tableData  ="";
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
            ids += selItems[i].ID + ",";
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
      newDept(){
        let _this = this;
        _this.ExpCode02 = "";
        _this.ExpName02 = "";
        _this.dialogFormVisible = true;
      },
      handleEdit2(index, row){
        var _this = this;

        _this.dialogFormVisible2 = true;
        _this.ExpCode02 = row.ExpCode;
        _this.ExpName02 = row.ExpName;
        _this.rowid02=row.ID;
        //console.log("BBBBBB");
      },
      update(){
        var _this = this;
        if (this.ExpCode02 == null || this.ExpCode02.length == 0
           || this.ExpName02 == null || this.ExpName02.length == 0)
        {
            _this.$message({
              type: 'info',
              message: '2项数据不能为空!'
            });
        } 
        else {
        
        putRequest("/neusys/expenseClass/update", {ID: this.rowid02, ExpCode: this.ExpCode02, 
         ExpName: this.ExpName02}).then(resp=> {
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
            _this.ExpCode02 = '';
            _this.ExpName02 = '';
            _this.refresh();
            _this.dialogFormVisible2 = false;
            
       }},
      handleDelete(index, row){
        let _this = this;
        this.$confirm('确认删除 "' + row.ExpName + '" ?', '提示', {
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
      deleteCate(ids){
        var _this = this;
        this.loading = true;
        //删除
        deleteRequest("/neusys/expenseClass/" + ids).then(resp=> {
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
              message: '该栏目下尚有文章，删除失败!'
            });
          }
        })
      },
      refresh(){
        let _this = this;
        _this.loading = false;
        getRequest("/neusys/expenseClass/all").then(resp=> {
          
          _this.tableData = resp.data.list;
          _this.totalCount = resp.data.totalCount;
          _this.currentPage01 = page ;
          
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
        //翻页相关 }

        ExpCode02: '',
        ExpName02: '',
        keywords: '',
        selItems: [],
        categories: [],
        tableData: [],
        loading: false,
        dialogAddVisible: false,
        dialogFormVisible: false,
        dialogFormVisible2: false,
        rowid02: '',
      }
    }
  }
</script>