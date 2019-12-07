<template>
  <el-container class="MaintinExcelFormat_lyc">
    <el-main > 
        <el-row>
            <el-button  size="mini" type="primary" @click="addExcel">添加</el-button>
            <el-button  size="mini" type="primary" :disabled="selectingRow == null" @click="editExcel" >编辑</el-button>
            <el-button  size="mini" type="danger" :disabled="selectingRow == null" @click="removeExcel" >删除</el-button>
        </el-row>
            <el-table ref="tableData" size="mini" highlight-current-row :data="tableData" @row-click="rowClick" @current-change="currentChange" stripe style="width: 100%;height=600px">
                <el-table-column prop="formatNo" label="模版编码" width="120">
                   <!-- <template slot-scope="scope">
                      <el-input size ="mini" v-model="scope.row.formatNo" placeholder="例‘默认模版’ "></el-input>
                    </template> -->
                </el-table-column>
                <el-table-column prop="prdNoPos" label="成品货号[坐标]" width="120">
                    <!-- <template slot-scope="scope">
                      <el-input size ="mini" v-model="scope.row.prdNoPos" placeholder="例‘A1’ "></el-input>
                    </template> -->
                </el-table-column>
                <el-table-column prop="deptNoPos" label="生产部门[坐标]" width="120">
                  <!-- <template slot-scope="scope">
                      <el-input size ="mini" v-model="scope.row.deptNoPos" placeholder="例‘A1’ "></el-input>
                  </template> -->
                </el-table-column>
                <el-table-column prop="idNoPos" label="配方号[坐标]" width="100">
                  <!-- <template slot-scope="scope">
                      <el-input size ="mini" v-model="scope.row.idNoPos" placeholder="例‘A1’ "></el-input>
                  </template>-->
                </el-table-column> 
                </el-table-column>
                <el-table-column prop="WHFormType" label="仓库取值" width="120">
                    <template slot-scope="scope">
                        <el-select size ="mini" :disabled=true v-model="scope.row.whFormType" placeholder="请选择">
                          <el-option key="1" value="1" label='1.货品默认仓库'></el-option>
                        </el-select>
                    </template></el-table-column>
                </el-table-column>
                <el-table-column prop="bomPlace" label="Bom储存位置" width="120">
                    <template slot-scope="scope">
                        <el-select size ="mini" :disabled=true v-model="scope.row.bomPlace" placeholder="请选择">
                          <el-option key="1" value="1" label='1.标准BOM'></el-option>
                          <el-option key="2" value="2" label='2.订单BOM'></el-option>
                        </el-select>
                    </template></el-table-column>
                </el-table-column>
                <el-table-column prop="startRow" label="数据开始行" width="100">
                  <!-- <template slot-scope="scope">
                      <el-input-number  size="mini" :precision=0 :controls=false  :min="1" v-model="scope.row.startRow" placeholder="例‘3’ "></el-input-number>
                  </template></el-table-column> -->
                </el-table-column>
                <el-table-column prop="bomStructCell" label="数据阶数列" width="80">
                  <!-- <template slot-scope="scope">
                      <el-input-number size="mini" :precision=0 :controls=false  :min="1" v-model="scope.row.bomStructCell" placeholder="例‘3’ "></el-input-number>
                  </template></el-table-column> -->
                </el-table-column>
                <el-table-column label="Bom" width="200">
                    <template slot-scope="scope">
                        <el-button size="mini" type="primary" @click="handleEdit(scope.$index, scope.row)">字段明细</el-button>
                    </template>
                </el-table-column>
            </el-table>
    </el-main>

    <!-- 弹出窗体Excel -->
    <el-dialog title="Excel模版" :visible.sync="dialogExcelVisible"  :close-on-click-modal=false> 
            <el-form ref="formExcel" :model="formExcel" :rules="formRules" label-width="120px" size="mini">
              <el-row type="flex">
                <el-col :span=12>
                    <el-form-item label="模版编码" style="font-size=8px" prop="formatNo">
                          <el-input size ="mini" :disabled="formExcel.id != -1" v-model="formExcel.formatNo" placeholder="例‘默认模版’ ">
                          </el-input>
                    </el-form-item>  
                </el-col>
                <el-col :span=12>
                  <el-form-item label="成品货号[坐标]" style="font-size=8px" prop="prdNoPos">
                        <el-input size ="mini" v-model="formExcel.prdNoPos" placeholder="例‘A1’ ">
                        </el-input>
                  </el-form-item>  
                </el-col>
              </el-row>

              <el-row type="flex">
                <el-col :span=12>
                    <el-form-item label="生产部门[坐标]" style="font-size=8px" prop="deptNoPos">
                          <el-input size ="mini" v-model="formExcel.deptNoPos" placeholder="例‘A1’ ">
                          </el-input>
                    </el-form-item>  
                </el-col>
                <el-col :span=12>
                    <el-form-item label="配方号[坐标]" style="font-size=8px" prop="idNoPos">
                          <el-input size ="mini" v-model="formExcel.idNoPos" placeholder="">
                          </el-input>
                    </el-form-item>  
                </el-col>
              </el-row>

              <el-row type="flex">
                <el-col :span=12>
                    <el-form-item label="仓库取值" style="font-size=8px" prop="whFormType">
                        <el-select label="仓库取值" size ="mini" v-model="formExcel.whFormType" placeholder="请选择">
                          <el-option key="1" value="1" label='1.货品默认仓库'></el-option>
                        </el-select>
                    </el-form-item>  
                </el-col>
                <el-col :span=12>
                    <el-form-item label="Bom储存位置" style="font-size=8px" prop="bomPlace">
                      <el-select size ="mini" v-model="formExcel.bomPlace" placeholder="请选择">
                            <el-option key="1" value="1" label='1.标准BOM'></el-option>
                            <el-option key="2" value="2" label='2.订单BOM'></el-option>
                          </el-select>
                      </el-form-item>  
                </el-col>
              </el-row>

              <el-row type="flex">
                <el-col :span=12>
                    <el-form-item label="数据开始行" style="font-size=8px" prop="startRow">
                        <el-input-number size="mini" :precision=0 :min="1" v-model="formExcel.startRow" placeholder="例‘3’ ">
                        </el-input-number>
                    </el-form-item>  
                </el-col>
                <el-col :span=12>
                     <el-form-item label="数据阶数列" style="font-size=8px" prop="bomStructCell">
                        <el-input-number size="mini" :precision=0 :min="1"   v-model="formExcel.bomStructCell"  placeholder="例‘3’ ">
                        </el-input-number>
                    </el-form-item>  
                </el-col>
              </el-row>
          </el-form>
        <el-row>
            <el-button size="mini" type="success" @click="beforeSaveExcel">保存</el-button>
            <el-button size="mini" type="" @click="cancelExcel">取消</el-button>
        </el-row>
    </el-dialog>

    <!-- 弹出窗体－字段明细 -->
    <el-dialog title="Bom表头" :visible.sync="dialogDtlVisible" :close-on-click-modal=false > 
        <el-row>
            <el-button  size="mini" type="primary" @click="insertDtlRow">插入</el-button>
            <el-button  size="mini" type="success" @click="saveDtls">保存</el-button>
        </el-row>

        <el-table size="mini":data="tableDataDtl" stripe style="width: 100%;height=600px">
          <el-table-column prop="cell_type" label="数据类型" width="120">
              <template slot-scope="scope">
                <!-- :disabled="scope.row.cell_type=='SYS' && scope.row.id > 0" -->
                  <el-select size ="mini"  v-model="scope.row.cell_type" placeholder="请选择">
                    <el-option key="SYS" value="SYS" label='SYS.系统保留'></el-option>
                    <el-option key="DIY" value="DIY" label='DIY.系统自定义'></el-option>
                    <el-option key="SHOW" value="SHOW" label='SHOW.显示字段'></el-option>
                  </el-select>
              </template>
          </el-table-column>
          <el-table-column prop="diy_type" label="DIY位置" width="120">
            <template slot-scope="scope">
                <el-select size ="mini" v-model="scope.row.diy_type" clearable placeholder="">
                  <el-option key="1" value="1" label='1.BOM表头自定义'></el-option>
                  <el-option key="2" value="2" label='2.BOM表身自定义'></el-option>
                  <el-option key="3" value="3" label='2.二者'></el-option>
                </el-select>
            </template>
        </el-table-column>
          <el-table-column prop="cell_index" label="列Y" width="60">
              <template slot-scope="scope">
                <el-input-number size="mini" :controls=false :precision=0 :min="1" v-model="scope.row.cell_index" placeholder="例‘3’ "></el-input-number>
              </template>
          </el-table-column>
          <el-table-column prop="field_no" label="字段编码" width="120">
              <template slot-scope="scope">
                <el-input size ="mini" v-model="scope.row.field_no" placeholder=""></el-input>
              </template>
          </el-table-column>
          <el-table-column prop="field_name" label="字段名" width="120">
              <template slot-scope="scope">
                <el-input size ="mini" v-model="scope.row.field_name" placeholder=""></el-input>
              </template>
          </el-table-column>
          <el-table-column prop="check_exist" label="检查资料" width="150">
              <template slot-scope="scope">
                <el-input size ="mini" v-model="scope.row.check_exist" placeholder=""></el-input>
              </template>
          </el-table-column>
          <el-table-column label="操作" width="200">
              <template slot-scope="scope">
                  <el-button size="mini" type="primary" @click="removeDtlRow(scope.$index, scope.row)">移除</el-button>
              </template>
          </el-table-column>
        </el-table>
    </el-dialog>
  </el-container>
   
</template>

<script>
const axios = require('axios').default;
// import Top from './Top.vue'
export default {
  name: 'HelloWorld',
  data () {
    return {
      formExcel:{
          id: -1,
          formatNo: "",
          prdNoPos:"",
          deptNoPos:"",
          idNoPos:"",
          whFormType:"1",
          bomPlace:"1",
          startRow:3,
          bomStructCell:2
      },
      formRules:{
        formatNo: [{ required: true, message: '请输入', trigger: 'blur' }],
        prdNoPos: [{ required: true, message: '请输入', trigger: 'blur' }],
        deptNoPos: [{ required: true, message: '请输入', trigger: 'blur' }]
      },
      dialogExcelVisible:false,
      dialogDtlVisible:false,
      selectingRow: null,
      tableData: [ ],
      tableDataDtl:[]
    }
  },
  components:{
  },
  mounted(){
    var me = this;
  },
  methods:{
     rowClick:function(row, column, event){
       
        this.selectingRow = row;
     },
     currentChange:function(){
        //console.log('current-change');
     },

     addExcel:function(){
        //console.log('addExcel');
        //console.log(this);
        this.dialogExcelVisible = true;
        this.dialogDtlVisible = false;

        this.formExcelEdit = false;
        this.formExcel.id = -1;
        this.formExcel.formatNo ="";
        this.formExcel.prdNoPos = "";
        this.formExcel.deptNoPos = "";
        this.formExcel.idNoPos = "";
        this.formExcel.whFormType = "1";
        this.formExcel.bomPlace = "1";
        this.formExcel.startRow = 3;
        this.formExcel.bomStructCell = 2;
     },
     editExcel:function(){
       var me =this;
        me.dialogExcelVisible = true;
        me.dialogDtlVisible = false;

         if(!me.selectingRow)
            return;
          
       
        //console.log(me.selectingRow);
        me.selectingRowData = JSON.parse(JSON.stringify(me.selectingRow));
        //console.log(b);
        //me.formExcel = me.selectingRowData;
         me.deepClone(me.formExcel, me.selectingRowData);
     },
     deepClone:function(targerObj,  obj){
          //定义对象来判断当前的参数是数组还是对象
          //let objClone = Array.isArray(obj)?[]:{};
          //如果obj存在并且为对象		
          if(obj&&typeof obj == "object"){
            for(let key in obj){
                
              if(obj.hasOwnProperty(key)){
                if(obj[key]&&typeof obj[key] == "object"){
                  //objClone[key] = deepClone(obj[key]);
                }else{
                //如果不是，直接赋值
                  targerObj[key] = obj[key];
                }
              }
            }
          }	
          return targerObj;
      } ,
     beforeSaveExcel:function(){
        var me = this;
        this.$refs["formExcel"].validate((valid) => {
            if (valid) {
              me.saveExcel();
            } else {
              return false;
            }
          });
     },
     saveExcel:function(){
       var me = this;
       let isNew = me.formExcel.id == -1 ? true :false;

       axios.post("http://localhost:61106/api/bom/SaveExcelFormat", me.formExcel)
          .then(function (response) {
            if(response.data.code == 200){
              me.formExcel = response.data.data;
              if(isNew == false){
                  me.deepClone(me.selectingRow, response.data.data);
                  //me.selectingRow = response.data.data;
              }
              me.onLoad();
              //me.formExcel.id = response.data.id;
              me.$message({
                message: '保存成功!',
                type: 'success'
              });
            }
            else{
                me.$message({
                  message: '失败:' + response.data.message,
                  type: 'warning'
                });
            }
          }).catch(function (error) {console.log(error);}).finally(function () {          });
     },
     removeExcel:function(){
       var me = this;
        axios.get("http://localhost:61106/api/bom/RemoveExcelFormat",  {
            params: {
              idxxx: me.selectingRow.id
            }
          })
          .then(function (response) {
            if(response.data.code == 200){
              if(response.data.data == true){
                let index =me.tableData.indexOf(me.selectingRow);
                me.tableData.splice(index, 1);
                me.selectingRow  = null;

                me.$message({
                  message: '删除成功!',
                  type: 'success'
                });
              }
              else
                me.$message({
                  message: '失败:' + response.data.message,
                  type: 'warning'
                });
            }
            else{
                me.$message({
                  message: '失败:' + response.data.message,
                  type: 'warning'
                });
            }
            // me.dialogExcelVisible = false;
          }).catch(function (error) {console.log(error);}).finally(function () {          });
     },
     cancelExcel:function(){
        //console.log(this.$refs["formExcel"]);
        this.$refs["formExcel"].resetFields();
        this.dialogExcelVisible = false;
     },
     handleEdit:function(index, row){
         var me = this;
        
        axios.get("http://localhost:61106/api/bom/GetDtls",{
            params:{
              HeaderId:row.id
            }
          })
          .then(function (response) {
              me.tableDataDtl = response.data.data;
              //console.log(me.tableDataDtl);

              me.dialogExcelVisible = false;
              me.dialogDtlVisible = true;

          }).catch(function (error) {console.log(error);}).finally(function () {});
     },
     insertDtlRow:function(){
        var me = this;
        me.tableDataDtl.push({
            id : -1,
            diy_type: "",
            cell_type:'SHOW',
        });
     },
     removeDtlRow:function(index, row){
        var me = this;
        me.tableDataDtl.splice(index, 1);
     },
     saveDtls:function(){
        let me = this;

        for(var i = 0;i< me.tableDataDtl.length; ++i){
          //console.log(me.tableDataDtl[i].fieldNo);
          if(me.tableDataDtl[i].cell_type == "DIY" && !me.tableDataDtl[i].diy_type ){
              me.$message({
                message: 'DIY字段，需要指定DIY位置!',
                type: 'warning'
              });
              return false;
          }
          
          if(!me.tableDataDtl[i].cell_type 
              || !me.tableDataDtl[i].cell_index  
              || !me.tableDataDtl[i].field_no 
              || !me.tableDataDtl[i].field_name){
              
               me.$message({
                message: '字段输入不完整!',
                type: 'warning'
              });
              return false;
          }

          me.tableDataDtl[i].field_no = me.tableDataDtl[i].field_no.toLowerCase();
        }

        axios.post("http://localhost:61106/api/bom/SaveBomFormatDetail",{
            HeaderId: me.selectingRow.id,
            Dtls: me.tableDataDtl
          })
          .then(function (response) {
              if(response.data.code == 200){
                me.$message({
                  message: '字段明细保存成功!',
                  type: 'success'
                });
                me.dialogDtlVisible = false;
              }
              else{
                  me.$message({
                    message: '失败:' + response.data.message,
                    type: 'warning'
                  });
              }
          }).catch(function (error) {console.log(error);}).finally(function () {});

     },
     onLoad:function(){
        var me = this;
        axios.get("http://localhost:61106/api/bom/SearchExcelFormatList")
          .then(function (response) {
              me.tableData = response.data.data;
          })
          .catch(function (error) {
            console.log(error);
          })
          .finally(function () {
          });
     }
  },
  mounted(){
      var me = this;
      //console.log('mounted');
      me.onLoad();
  }
}
</script> 


<style scoped>
  .MaintinExcelFormat_lyc /deep/ .el-input-number--mini {
      width: 50px;
      line-height: 26px;
  }
 
</style>
