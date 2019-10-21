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
          width="55" align="left">
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

         <el-table-column
          v-if='false'
          label="Id"
          prop="Id"
          width="120" align="left">
        </el-table-column>
       
        <el-table-column label="操作" align="left">
          <template slot-scope="scope">
            <!--
            <el-button type="primary"
              size="mini"
              @click="handleEdit(scope.$index, scope.row)">编辑0
            </el-button>
            -->
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
            <el-col :span="12">
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

<!-- 常数项管理 弹框---start -->
   <el-dialog title="常数项列表" :visible.sync="dialogAddVisible" width="750px" align="center">

   <el-input
        placeholder="请输入常数项编码或名称" size="mini"
        v-model="keywords202" style="width: 200px;">
   </el-input>
    
   <el-button type="primary" size="mini" style="margin-left: 10px" @click="searchClick202">查询常数项</el-button>

   <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept3()">新增常数项</el-button>

   <el-button type="primary" size="mini" style="margin-left: 10px" icon="el-icon-edit" @click="newDept4()">新增常数项000</el-button>
  
 
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
          label="常数项ID"
          prop="Id"
          width="120" align="left" >
        </el-table-column>

        <el-table-column label="操作" align="left">
          <template slot-scope="scope">
            <el-button type="primary"
              size="mini"
              @click="">编辑
            </el-button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete202(scope.$index, scope.row)">删除
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
<!-- 常数项管理弹框---end -->

<!-- 新增人员弹框---start -->
    <el-dialog title="新增人员9999" :visible.sync="dialogFormVisible9" width="550px" align="center">
      
      <el-input
        placeholder="请输入姓名"
        v-model="personName" style="width: 200px;">
      </el-input>

      <el-input
        placeholder="请输入身份证号码"
        v-model="sfzh" style="width: 200px;">
      </el-input>
     
      <el-select v-model="nid" placeholder="请选择民族" style="width: 150px;">
             <el-option
                v-for="item in categories3"
                :key="item.id"
                :label="item.nationName"
                :value="item.id">
              </el-option>
      </el-select>
      <!--  cityName  ConstantTypeName -->
      <el-select v-model="cid" placeholder="请选择城市" style="width: 150px;">
             <el-option
                v-for="item in categories2"
                :key="item.Id"
                :label="item.ConstantTypeName"
                :value="item.Id">
              </el-option>
      </el-select>
  <el-main class="cate_mana_header">
     <el-button type="primary" size="medium" style="margin-left: 10px" @click="addNewCate">新增人员</el-button>
  </el-main>
</el-dialog>
<!-- 新增人员弹框---end -->

</gao-el-container>
</template>

<script>
  import {postRequest} from '../../utils/api'
  import {putRequest} from '../../utils/api'
  import {deleteRequest} from '../../utils/api'
  import {getRequest} from '../../utils/api'
  export default{
    methods: {
      
      handleSelectionChange(val) {
        this.selItems = val;
      },
      handleSelectionChange202(val) {
        this.selItems202 = val;
      },
      newDept(){
        let _this = this;
        this.dialogFormVisible = true;
      },
      newDept2(){
        let _this = this;
        this.dialogAddVisible = true;
      },
      //翻页相关 
      searchClick(){
        this.loadBlogs(1, this.pageSize);
      },
      searchClick202(){
        this.loadBlogs202(1, this.pageSize202);
      },
      //翻页相关  
      loadBlogs(page, count){
        var _this = this;
        var url = "/ucsys/constantType/all?page=" + page + "&count=" + count + "&keywords=" + this.keywords + "&keywords2=" + this.keywords2;
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
          //压根没见到服务器
          _this.loading = false;
          _this.$message({type: 'error', message: '数据加载失败!'});
        })
      },
       loadBlogs202(page, count){
        var _this = this;
        var url = "/ucsys/constantItem/all?page=" + page + "&count=" + count + "&keywords=" + this.keywords202;
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
      //翻页   //翻页相关 
      currentChange(currentPage){
        this.currentPage01 = currentPage;
        this.loading = true;
        this.loadBlogs(currentPage, this.pageSize);
      },
      //翻页   //翻页相关 
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
        this.dialogFormVisible9 = true;
      },
      refresh(){
        let _this = this;
        getRequest("/ucsys/constantType/all").then(resp=> {
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

        getRequest("/ucsys/constantItem/all").then(resp=> {
          //_this.categories = resp.data;
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

        getRequest("/gj_admin/nation/all").then(resp=> {
          _this.categories3 = resp.data;
        });

        //  ucsys/constantType/all   |  gj_admin/city/all
        getRequest("/ucsys/constantType/all").then(resp=> {
          //_this.categories2 = resp.data;
          _this.categories2 = resp.data.list;
        });
      }
    },
  
    
    mounted: function () {

     // this.getCategories();
     // this.getCategories2();

      this.loading = true;
      this.refresh();

    
    },
    data(){
      return {
        //翻页相关 { 
        totalCount: 2,
        currentPage01:1,
        pageSize: 6,
        keywords: '',
        keywords2: '',

       

        totalCount202: 2,
        currentPage202:1,
        pageSize202: 6,
        keywords202: '',
       
        
        nid111: '',
        //翻页相关 }

        cityName: '',
        selItems: [],
        tableData:[],
        tableData2:[],
        categories: [],
       
        categories202: [],
        selItems202: [],
        loading: false,
        
        dialogAddVisible: false,
        dialogFormVisible: false,
        dialogFormVisible2: false,
        dialogFormVisible3: false,
        dialogFormVisible5: false,

        dialogFormVisible9: false,

        rowid2:'',
        ConstantTypeCode2:'',
        ConstantTypeName2:'',

        personName: '',
        categories2: [],
        categories3: [],
        nid: '',
        cid: '',
        nid2: '',
        cid2: '',
        personName2: '',
        sfzh2: '',
        rowid2:''
        
         
      }
    }
  }
</script>

