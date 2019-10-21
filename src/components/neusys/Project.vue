<template>
   <gao-el-container>

<el-row :gutter="2">
      <el-col :span="5">
      <div class="grid-content bg-purple">
           <el-input
        placeholder="请输入非药品收费项目编码或名称" size="mini"
        v-model="keywords" style="width: 216px;">
      </el-input>
      </div>
      </el-col>

      <el-col :span="4">
      <div class="grid-content bg-purple">
           <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-view" @click="searchClick">查询非药品收费项目</el-button>
      </div>
      </el-col>

      <el-col :span="4">
      <div class="grid-content bg-purple">
           <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept()">新增非药品收费项目</el-button>
      </div>
      </el-col>

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
            <!--
            <el-button size="mini" type="primary" icon="el-icon-sold-out">导入非药品收费项目</el-button>
            -->
          </el-upload>
      </div>
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
          width="55" align="left">
        </el-table-column>
        <el-table-column
          label="项目编码"
          prop="ItemCode"
          width="120" align="left">
        </el-table-column>
        <el-table-column
          label="项目名称"
          prop="ItemName"
          width="120" align="left">
        </el-table-column>

         <el-table-column
          label="项目类型"
          prop="RecordType"
          width="120" align="left" >
  <template slot-scope="scope">
              {{ scope.row.RecordType|showScope }}
  </template>
        </el-table-column>

        <el-table-column
          label="规格"
          prop="Format"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="单价"
          prop="Price"
          width="120" align="left">
        </el-table-column>

        <el-table-column
          label="费用科目"
          prop="ExpName"
          width="120" align="left">
        </el-table-column>
        <el-table-column
          label="费用分类2"
          prop="ecid"
          width="120" align="left" v-if='false'>
        </el-table-column>

        <el-table-column
          label="执行科室"
          prop="DeptName"
          width="120" align="left">
        </el-table-column>

        <!-- v-if='false' -->
        <el-table-column
          label="执行科室2"
          prop="dpid"
          width="120" align="left" v-if='false' >
        </el-table-column>

        <el-table-column
          label="MnemonicCode"
          prop="MnemonicCode"
          width="120" align="left" v-if='false' >
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
            <el-col :span="6">
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

    </gao-el-main>

<!-- 新增非药品收费项目弹框---start -->
        <el-dialog title="新增非药品收费项目" :visible.sync="dialogFormVisible" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
               <el-row :gutter="2">
      
                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">项目编码:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                                <el-input v-model="ItemCode02" placeholder="项目编码" size="mini"></el-input>
                      </el-form-item>
                  </div>
                  </el-col>

                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">项目名称:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                                <el-input v-model="ItemName02" placeholder="项目名称" size="mini"></el-input>
                      </el-form-item>
                  </div>
                  </el-col>
              </el-row> 
              <el-row :gutter="2">
      
                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">规格:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                                <el-input v-model="Format02" placeholder="规格" size="mini"></el-input>
                      </el-form-item>
                  </div>
                  </el-col>

                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">单价:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                                <el-input v-model="Price02" placeholder="单价" size="mini"></el-input>
                      </el-form-item>
                  </div>
                  </el-col>
              </el-row> 

              <el-row :gutter="2">
      
                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">费用分类:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                    <el-select  placeholder="费用分类"  v-model="ExpClassID02" size="mini" >
                     <el-option
                        v-for="item in categories2"
                        :key="item.ID"
                        :label="item.ExpName"
                        :value="item.ID">
                      </el-option>

                    </el-select>
                </el-form-item>
                </div>
                </el-col>

                <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">执行科室:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                     <el-form-item label="">
                    <el-select  placeholder="执行科室" size="mini" v-model="DeptID02">
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
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">项目类型:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                    <el-select  placeholder="项目类型"  v-model="RecordType02" size="mini" >
                         <el-option label="检查" value="1"></el-option>
                         <el-option label="检验" value="2"></el-option>
                         <el-option label="处置" value="3"></el-option>
                    </el-select>
                </el-form-item>
                </div>
                </el-col>

                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">拼音助记码:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                                <el-input v-model="MnemonicCode02" placeholder="拼音助记码" size="mini"></el-input>
                      </el-form-item>
                  </div>
                  </el-col>
              </el-row> 
           </el-form>

            <br>
            <el-button type="warning" @click="dialogFormVisible = false" size="mini">取 消</el-button>
            <el-button type="primary" @click="addNewCate()" size="mini">保 存</el-button>
        </el-dialog>
<!-- 新增非药品收费项目弹框---end -->

<!-- 修改非药品收费项目弹框---start -->
        <el-dialog title="修改非药品收费项目" :visible.sync="dialogFormVisible2" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
               <el-row :gutter="2">
      
                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">项目编码:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                                <el-input v-model="ItemCode02" placeholder="项目编码" size="mini"></el-input>
                      </el-form-item>
                  </div>
                  </el-col>

                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">项目名称:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                                <el-input v-model="ItemName02" placeholder="项目名称" size="mini"></el-input>
                      </el-form-item>
                  </div>
                  </el-col>
              </el-row> 
              <el-row :gutter="2">
      
                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">规格:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                                <el-input v-model="Format02" placeholder="规格" size="mini"></el-input>
                      </el-form-item>
                  </div>
                  </el-col>

                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">单价:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                                <el-input v-model="Price02" placeholder="单价" size="mini"></el-input>
                      </el-form-item>
                  </div>
                  </el-col>
              </el-row> 

              <el-row :gutter="2">
      
                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">费用分类:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                    <el-select  placeholder="费用分类"  v-model="ExpClassID02" size="mini" >
                     <el-option
                        v-for="item in categories2"
                        :key="item.ID"
                        :label="item.ExpName"
                        :value="item.ID">
                      </el-option>
                     </el-select>
                </el-form-item>
                </div>
                </el-col>

                <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">执行科室:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                     <el-form-item label="">
                    <el-select  placeholder="执行科室" size="mini" v-model="DeptID02">
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
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">项目类型:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                    <el-select  placeholder="项目类型"  v-model="RecordType02" size="mini" >
                         <el-option label="检查" :value="1"></el-option>
                         <el-option label="检验" :value="2"></el-option>
                         <el-option label="处置" :value="3"></el-option>
                    </el-select>
                </el-form-item>
                </div>
                </el-col>

                  <el-col :span="4">
                  <div class="grid-content bg-purple">
                      <font style="font-size:13px;font-family:'Microsoft YaHei'">拼音助记码:</font>&nbsp;
                  </div>
                  </el-col>

                  <el-col :span="7">
                  <div class="grid-content bg-purple">
                      <el-form-item label="">
                                <el-input v-model="MnemonicCode02" placeholder="拼音助记码" size="mini"></el-input>
                      </el-form-item>
                  </div>
                  </el-col>
              </el-row> 
            </el-form>

            <br>
            <el-button type="warning" @click="dialogFormVisible2 = false" size="mini">取 消</el-button>
            <el-button type="primary" @click="update()" size="mini">保 存</el-button>
        </el-dialog>
<!-- 修改非药品收费项目弹框---end -->

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
         if (this.ItemCode02 == null || this.ItemCode02.length == 0
           ||this.ItemName02 == null || this.ItemName02.length == 0
           ) {
            _this.$message({
              type: 'info',
              message: '前2项数据不能为空!'
            });
          } 
          else {
            this.loading = true;
            postRequest('/neusys/fmeditem/add', {ItemCode: this.ItemCode02, ItemName: this.ItemName02,Format: this.Format02,Price: this.Price02,ExpClassID: this.ExpClassID02, DeptID: this.DeptID02,RecordType:this.RecordType02,MnemonicCode:this.MnemonicCode02}).then(resp=> {
            if (resp.status == 200) {
              var json = resp.data;
              _this.$message({type: json.status, message: json.msg});
              _this.ItemCode02 = '';
              _this.ItemName02 = '';
              _this.Format02 = '';
              _this.Price02 = '';
              _this.ExpClassID02 = '';
              _this.DeptID02 = '';
              _this.RecordType02 = "";
              _this.MnemonicCode02 = "";
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
      searchClick(){
          //this.loadBlogs(1, 100);//临时不翻页的情况
          this.loadBlogs(1, this.pageSize);
      },
      //翻页相关  
      loadBlogs(page, count){
        var _this = this;
        var url = "/neusys/fmeditem/all?page=" + page + "&count=" + count + "&keywords=" + this.keywords;
        getRequest(url).then(resp=> {
        if (resp.status == 200) {
            _this.loading = false;
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
      handleEdit2(index, row){
        var _this = this;
        _this.dialogFormVisible2 = true;
        _this.ItemCode02 = row.ItemCode;
        _this.ItemName02 = row.ItemName;  
        _this.Format02   = row.Format;  
        _this.Price02    = row.Price; 
        _this.MnemonicCode02 = row.MnemonicCode; 

        _this.ExpClassID02 = row.ecid;
        
        _this.DeptID02 = row.dpid;
        

        //_this.DeptID02 = row.DeptName;//DeptID

        _this.RecordType02 = row.RecordType;
        
        _this.rowid02=row.ID;
        //console.log("BBBBBB");
      },
      update(){
        var _this = this;
        if (this.ItemCode02 == null || this.ItemCode02.length == 0
           || this.ItemName02 == null || this.ItemName02.length == 0)
        {
            _this.$message({
              type: 'info',
              message: '2 项数据不能为空!'
            });
        } 
        else {
        
        putRequest("/neusys/fmeditem/update", {ID: this.rowid02, ItemCode: this.ItemCode02, 
         ItemName: this.ItemName02, Format: this.Format02, Price: this.Price02, ExpClassID: this.ExpClassID02, DeptID: this.DeptID02,RecordType:this.RecordType02,MnemonicCode:this.MnemonicCode02}).then(resp=> {
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
            _this.ItemCode02 = '';
            _this.ItemName02 = '';
            _this.Format02 = '';
            _this.Price02 = '';
            _this.ExpClassID02 = '';
            _this.DeptId02 = '';
            _this.RecordType02 = "";
            _this.MnemonicCode02 = "";
            _this.refresh();
            _this.dialogFormVisible2 = false;
        }},
        handleDelete(index, row){
          let _this = this;
          this.$confirm('确认删除 "' + row.ItemName + '" ?', '提示', {
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
        _this.ItemCode02 = "";
        _this.ItemName02 = "";
        _this.Format02   = "";
        _this.Price02    = "";
        _this.ExpClassID02 = "";
        _this.DeptID02 = "";
        _this.RecordType02 = "";
        _this.MnemonicCode02 = "";
        _this.dialogFormVisible = true;
      },
      deleteCate(ids){
        var _this = this;
        this.loading = true;
        //删除
        deleteRequest("/neusys/fmeditem/" + ids).then(resp=> {
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
        _this.loading = false;
        getRequest("/neusys/fmeditem/all").then(resp=> {
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

        getRequest("/neusys/department/all?page=1&count=200").then(resp=> {
            _this.categories3 = resp.data.list;
        }); 

        getRequest("/neusys/expenseClass/all?page=1&count=100").then(resp=> {
            _this.categories2 = resp.data.list;
        }); 
      }
    },
    mounted: function () {
      this.loading = true;
      this.refresh();
    },
     filters:{
      showScope:function(scope){
        if(scope=='1'){
          return '检查';
        }else if(scope=='2'){
          return '检验';
        }else if(scope=='3'){
          return '处置';
        }
      }
    },
    data(){
      return {

        //翻页相关 { 
        totalCount: 2,
        currentPage01:1,
        pageSize: 10,
        //翻页相关 }
        
        keywords: '',
        ExpClassID02:'',
        DeptID02: '',
        RecordType02: '',
        MnemonicCode02: '',
        selItems: [],
        categories: [],
        categories3: [],
        categories2: [],
        tableData: [],
        loading: false,
        dialogAddVisible: false,
        dialogFormVisible: false,
        dialogFormVisible2: false,
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