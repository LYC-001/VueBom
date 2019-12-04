<template>
  <el-container class="elmain_lyc">
    <el-aside style="background-color: rgb(1, 241, 246);padding=5px" >
      <el-form ref="form" label-width="75px" size="medium">
        <el-upload
              width="100px"
              :show-file-list=false 
              class="upload-demo"
              drag
              action="https://jsonplaceholder.typicode.com/posts/">
              <i class="el-icon-upload"></i>
              <div class="el-upload__text">Excel Bom文件拖到此处，或<em>点击上传</em></div>
        </el-upload>
      
        <el-form-item label="成品代号" style="font-size=8px">
              <el-input v-model="form.bomPrdNo" placeholder="请输入内容"></el-input>
        </el-form-item>    
        <el-form-item label="成品名称">
              <el-input v-model="form.bomPrdName" placeholder="请输入内容"></el-input>
        </el-form-item>
        <el-form-item label="制造部门">
              <el-input v-model="form.bomDeptNo" placeholder="请输入内容"></el-input>
        </el-form-item>   
         <el-form-item label="导入模版" >
            <el-row type="flex" :gutter="20">
              <el-col :span="14">
                  <el-select v-model="Format" filterable  placeholder="请选择">
                    <el-option
                      v-for="item in ExcelFormats"
                      :key="item.id"
                      :label="item.formatNo"
                      :value="item.id">
                      <span style="float: left">{{ item.formatNo }}</span>
                    </el-option>
                </el-select>
              </el-col>
            <el-col size="mini" :span="4">
              <el-button>维 护</el-button>

            </el-col>
            </el-row>
            
        </el-form-item>   
        <el-form-item>
          <el-button type="primary" icon="el-icon-right">导入ERP</el-button> 
        </el-form-item>
      </el-form>
    </el-aside >

    <el-main > 
      <el-table
            style="width: 100%">
            <el-table-column v-for="col in cols" :prop="col.prop" width="180" :label="col.label"></el-table-column>
      </el-table>
    </el-main>
  </el-container>
</template>

<script>
const axios = require('axios').default;
// import Top from './Top.vue'
export default {
  name: 'HelloWorld',
  data () {
    return {
      dialogVisible:true,
      Format:'SYS',
      ExcelFormats:[
        {
          Id: 'SYS',
          formatNo: '标准模版'
        }, {
          Id: 'User_01',
          formatNo: '用户自定义1'
        }, {
          Id: 'User_02',
          formatNo: '用户自定义2'
        }
      ],
      cols:[{
        prop: 'date',
        label: '日期'
      },
      {
        prop: 'name',
        label: '姓名'
      },
      {
        prop: 'address',
        label: '地址'
      }],
      form:{
        bomPrdNo:'AP--00002',
        bomPrdName:'AP--名称',
        bomDeptNo:'003'
      }
    }
  },
  components:{
    
  },
  
  methods:{
      showList(){
      }
  },
  
  mounted(){
    var me = this;
   
    me.showList();
    axios.get("http://localhost:61106/api/bom/SearchExcelFormatList")
          .then(function (response) {
              me.ExcelFormats = response.data.data;
              console.log(response);
              console.log(me.ExcelFormats);
          })
          .catch(function (error) {

            console.log(error);
          })
          .finally(function () {

          });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.elmain_lyc /deep/ .el-main{
  padding:0px;
  padding-left: 10px;
}

 .elmain_lyc /deep/ .el-upload-dragger{
  width: 280px;
}
</style>
