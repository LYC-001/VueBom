<template>
  <el-container class="elmain_lyc">
    <el-aside style="background-color: rgb(1, 241, 246);padding=5px" >
      <el-form ref="form" label-width="70px" size="mini">
         <el-row type="flex" >
          <el-col :span = 10>
              <el-upload
                    :show-file-list=false 
                    class="upload-demo"
                    drag
                    :data="UploadWithData"
                    :action="ExcelUrl"
                    :on-success="onUploadSuccess"
                    :on-error="onUploadError">
                    <i class="el-icon-upload"></i>
                    <div class="el-upload__text">Excel Bom文件拖到此处，或<em>点击上传</em></div>
              </el-upload>  
          </el-col>
        </el-row>
        

        <el-row  >
          <el-col :span = 16>
              <el-form-item label="成品代号" style="font-size=8px">
                    <el-input v-model="form.bomPrdNo" placeholder="请输入内容"></el-input>
              </el-form-item>    
          </el-col>
        </el-row>

        <el-row>
          <el-col :span = 16>
            <el-form-item label="成品名称">
                  <el-input v-model="form.bomPrdName" placeholder="请输入内容"></el-input>
            </el-form-item>
           </el-col>
        </el-row>
        
        <el-row>
          <el-col :span = 16>
              <el-form-item label="制造部门">
                <el-input v-model="form.bomDeptNo" placeholder="请输入内容"></el-input>
              </el-form-item> 
           </el-col>
        </el-row>
         <el-row>
          <el-col :span = 16>
             <el-form-item label="导入模版" >
                <el-select v-model="SelectFormatNo" filterable  placeholder="请选择" @change="FormatChange">
                      <el-option
                        v-for="item in ExcelFormats"
                        :key="item.id"
                        :label="item.formatNo"
                        :value="item.id">
                        <span style="float: left">{{ item.formatNo }}</span>
                      </el-option>
                </el-select>   
            </el-form-item>   
           </el-col>
        </el-row>
  
        <el-form-item>
          <el-button type="primary" icon="el-icon-right">导入ERP</el-button> 
        </el-form-item>
      </el-form>
    </el-aside >

    <el-main > 
      <el-table ref="BomData" :data="BomData"  style="width: 100%" >
            <el-table-column v-for="col in cols" :prop="col.prop" width="180" :label="col.label" :width="col.gridWidth"></el-table-column>
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
      UploadWithData:{
        HeaderId: '123445689'
      },
      ExcelUrl: 'http://localhost:61106/api/bom/LoadExcel?1=1',
      dialogVisible:true,
      SelectFormatNo:'',
      ExcelFormats:[],
      cols:[{
        prop: 'date',
        label: '先选择[导入模版]'
      }],
      BomData:[],
      form:{
        bomPrdNo:'',
        bomPrdName:'',
        bomDeptNo:'003'
      }
    }
  },
  components:{
    
  },
  
  methods:{
      FormatChange(selKey){
          //console.log(arguments);
          var me = this;
          let formatNo = '';
          for (let index = 0; index < me.ExcelFormats.length; ++index) {
            const element = me.ExcelFormats[index];
             if(element.id == selKey){
               formatNo = selKey.formatNo;
               break;
            }
          }

          me.ReLyaoutTable(selKey);
          me.ExcelUrl = "http://localhost:61106/api/bom/LoadExcel?1=1&formatNo="+ formatNo;
      },
      ReLyaoutTable(FormatHeaderId){
         var me = this;
          //加载字段列表 axios
          axios.get("http://localhost:61106/api/bom/GetDtls",{
                  params:{
                    HeaderId: FormatHeaderId,
                  }
                }).then(function (response) {
                if(response.data.code == 200){
                  var listCols = [
                      {
                        prop: 'bom_level',
                        label: '阶数',
                        gridWidth : 60,
                      },
                      {
                        prop: 'check_result',
                        label: '检测',
                        gridWidth : 100
                      }
                  ];
                   var dtls = response.data.data;
                    for (let index = 0; index < dtls.length; index++) {
                      const dtl = dtls[index];
                      listCols.push({
                        prop: dtl.field_no,
                        label: dtl.field_name
                      });
                    }

                    me.cols = listCols;
                }
              })
              .catch(function (error) {
                console.log(error);
              })
              .finally(function () {

              });
          //同步cols字段 
      },
      onUploadSuccess(response, file, fileList){
          var me = this;
          if(response.code == 200){
            var responseData = response.data;
            me.BomData = response.data.boms;
            console.log(me.BomData);
            
            me.form.bomPrdNo = responseData.prd_no;
            me.form.bomDeptNo = responseData.dep_no;
            me.$message({
              message: '加载Excel成功!',
              type: 'success'
            }); 
          }
          else{
            me.$message({
              message: '加载Excel成功失败:' + response.message,
              type: 'warning'
            });
          }
      },
      onUploadError(err, file, fileList){
          var me = this;
          me.$message({
            message: '上传失败:网络异常',
            type: 'warning'
          });
      },
      showList(){
      }
  },
  
  mounted(){
    var me = this;
   
    me.showList();
    axios.get("http://localhost:61106/api/bom/SearchExcelFormatList")
          .then(function (response) {
             if(response.data.code == 200){
                me.ExcelFormats = response.data.data;
                if(me.ExcelFormats.length > 0)
                  me.SelectFormatNo = me.ExcelFormats[0].formatNo;
                  me.ExcelUrl = "http://localhost:61106/api/bom/LoadExcel?1=1&formatNo="+ me.ExcelFormats[0].formatNo;

                  me.ReLyaoutTable(me.ExcelFormats[0].id);
             }
              // console.log(response);
              // console.log(me.ExcelFormats);
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
   align-content: space-between;
   margin-left: 10px; 
  width: 280px;
}
</style>
