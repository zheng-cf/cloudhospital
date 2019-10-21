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

  

</gao-el-container> 
</template>

<script>
  import {postRequest} from '../../utils/api'
  import {putRequest} from '../../utils/api'
  import {deleteRequest} from '../../utils/api'
  import {getRequest} from '../../utils/api'
  export default{
    methods: {
    
    mounted: function () {
      this.loading = true;
      this.refresh();
    },


  searchClick(){

       
        var users = [
          {name: "张XX", "email": "zhang@email.com"},
          {name: "江XX",   "email": "jiang@email.com"},
          {name: "李XX",  "email": "li@email.com"}
        ];
         
        var emails = users.map(function (user) { return user.name +"|"+ user.email; });
 
        console.log(emails.join(","));



    /*
        let obj1 = {a:1}, obj2 = {b:2}; 
            const map = new Map([
                ['name','张三'],
                ['age',22],
                ['sex','男'],
                [obj1,'今天天气好'],
                [[1,2],'哈哈哈'],
                ['age',55]
            ]); 
            console.log(map);
            console.log(map.size);//5 也可以用来去重

               map.forEach(function(value,index){
                console.log(index + ': ' + value);
            })
    */
    },
    data(){
     
      return {
         
        
         dialogAddVisible: false,
         dialogFormVisible: false,
        
      }

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