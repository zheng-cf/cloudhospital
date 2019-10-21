<template>

  <gao-el-container>
      <el-input
        placeholder="请输入结算类别编码或名称"  size="mini"
        v-model="keywords" style="width: 200px;">
      </el-input>
      <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-view" @click="searchClick">查询结算类别</el-button>

      <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept()">新增结算类别</el-button>
    </el-header>
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
          width="55" align="left" v-if='false'>
        </el-table-column>

        <el-table-column
          label="显示顺序号"
          prop="SequenceNo"
          width="120" align="left">
        </el-table-column>
        
        <el-table-column
          label="类别编码"
          prop="SettleCode"
          width="120" align="left">
        </el-table-column>
        <el-table-column
          label="类别名称"
          prop="SettleName"
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
      <el-button type="danger" :disabled="this.selItems.length==0" style="margin-top: 10px;width: 100px;" size="mini"
                 @click="deleteAll" v-if="this.categories.length>0">批量删除
      </el-button>
    </gao-el-main>

<!-- 新增结算类别弹框---start -->
        <el-dialog title="新增结算类别" :visible.sync="dialogFormVisible" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
               
              <el-row :gutter="2">
                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">类别编码:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                           <el-input v-model="SettleCode02" placeholder="类别编码" size="mini"></el-input>
                       </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">类别名称:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                         <el-form-item label="">
                             <el-input v-model="SettleName02" placeholder="类别名称" size="mini"></el-input>
                         </el-form-item>
                    </div>
                    </el-col>
                </el-row>
                <el-row :gutter="2">
                  <el-col :span="4">
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
            <el-button type="primary" @click="addNewCate()" size="mini">保 存</el-button>
            
        </el-dialog>
 <!-- 新增结算类别弹框---end -->

 <!-- 修改结算类别弹框---start -->
        <el-dialog title="修改结算类别" :visible.sync="dialogFormVisible2" width="550px" align="center">
            <el-form :label-position="labelPosition" :label-width="labelWidth" :inline="true" :model="formEdit" class="demo-form-inline" size="mini">
               
              <el-row :gutter="2">
                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">类别编码:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                       <el-form-item label="">
                           <el-input v-model="SettleCode02" placeholder="类别编码" size="mini"></el-input>
                       </el-form-item>
                    </div>
                    </el-col>

                    <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <font style="font-size:13px;font-family:'Microsoft YaHei'">类别名称:</font>&nbsp;
                    </div>
                    </el-col>

                    <el-col :span="7">
                    <div class="grid-content bg-purple">
                         <el-form-item label="">
                             <el-input v-model="SettleName02" placeholder="类别名称" size="mini"></el-input>
                         </el-form-item>
                    </div>
                    </el-col>
                </el-row>
                <el-row :gutter="2">
                  <el-col :span="4">
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
            <el-button type="warning" @click="dialogFormVisible2 = false" size="mini">取 消</el-button>
            <el-button type="primary" @click="update()" size="mini">保 存</el-button>
            
        </el-dialog>
 <!-- 修改结算类别弹框---end -->
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
        if (   this.SettleCode02 == null || this.SettleCode02.length == 0
            || this.SettleName02 == null || this.SettleName02.length == 0
            || this.SequenceNo02 == null || this.SequenceNo02.length == 0
 
        ) {
            _this.$message({
              type: 'info',
              message: '数据不能为空!'
            });
          } 
          else {
            this.loading = true;
            //ucsys=01  neusys=02
            postRequest('/neusys/settleCategory/add', {SettleCode: this.SettleCode02, SettleName: this.SettleName02, SequenceNo: this.SequenceNo02,}).then(resp=> {
            if (resp.status == 200) {
              var json = resp.data;
              _this.$message({type: json.status, message: json.msg});
              _this.SettleCode02 = '';
              _this.SettleName02 = '';
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
      searchClick(){
        this.loadBlogs(1, 100);//临时不翻页的情况
      },
      //翻页相关  
      loadBlogs(page, count){
        var _this = this;
        //neusys=02 ucsys=01
        var url = "/neusys/settleCategory/all?page=" + page + "&count=" + count + "&keywords=" + this.keywords;
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
        _this.SettleCode02 = row.SettleCode;
        _this.SettleName02 = row.SettleName;
        _this.SequenceNo02 = row.SequenceNo;

       _this.rowid02=row.ID;
        //console.log("BBBBBB");
      },
      update(){
        var _this = this;
        if (this.SettleCode02 == null || this.SettleCode02.length == 0
           || this.SettleName02 == null || this.SettleName02.length == 0
           || this.SequenceNo02 == null || this.SequenceNo02.length == 0 
           )
        {
            _this.$message({
              type: 'info',
              message: '3 项数据不能为空!'
            });
        } 
        else {
        //01=ucsys 02=neusys
        putRequest("/neusys/settleCategory/update", {ID: this.rowid02, SettleCode: this.SettleCode02, 
         SettleName: this.SettleName02, SequenceNo: this.SequenceNo02}).then(resp=> {
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
            _this.SettleCode02 = '';
            _this.SettleName02 = '';
            _this.SequenceNo02 = '';
            _this.refresh();
            _this.dialogFormVisible2 = false;
            
       }},
      handleDelete(index, row){
        let _this = this;
        this.$confirm('确认删除 "' + row.SettleName + '" ?', '提示', {
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
        _this.SettleCode02 = "";
        _this.SettleName02 = "";
        _this.SequenceNo02 = "";
        _this.dialogFormVisible = true;
      },
      deleteCate(ids){
        var _this = this;
        this.loading = true;
        //删除   02=neusys   01=ucsys
        deleteRequest("/neusys/settleCategory/" + ids).then(resp=> {
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
        //02=neusys  01=ucsys
        getRequest("/neusys/settleCategory/all?count=100").then(resp=> {
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
      }
    },
    mounted: function () {
      this.loading = true;
      this.refresh();
    },
    data(){
      return {
        cityName: '',
        keywords: '',
        selItems: [],
        categories: [],
        tableData: [],
        loading: false,
         formAdd: {
                //表单对象
                name: "",
                address: "",
                date: "",
                other: ""
         },
         formEdit: {
                //表单对象
                name: "",
                address: "",
                date: "",
                other: ""
            },
         dialogAddVisible: false,
         dialogFormVisible: false,
         dialogFormVisible2: false,
         rowid02:'',
         /*
          tableData: [{
          id: 'js01',
          cityName: '自费',
          },{
          id: 'js02',
          cityName: '市医保'},
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