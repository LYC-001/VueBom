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
    <el-dialog title="Excel模版" :visible.sync="dialogExcelVisible" :close-on-click-modal=false> 
            <el-form ref="formExcel" :model="formExcel" label-width="120px" size="mini">
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
                          <el-input size ="mini" v-model="formExcel.idNoPos" placeholder="例‘A1’ ">
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
            <el-button size="mini" type="success" @click="saveExcel">保存</el-button>
            <el-button size="mini" type="" @click="cancelExcel">取消</el-button>
        </el-row>
    </el-dialog>

    <!-- 弹出窗体－字段明细 -->
    <el-dialog title="Bom表头" :visible.sync="dialogBomHeaderVisible" :close-on-click-modal=false > 
        <el-row>
            <el-button  size="mini" type="primary" >添加</el-button>
            <el-button  size="mini" type="danger">删除</el-button>
            <el-button  size="mini" type="success">保存</el-button>
        </el-row>
        <el-table
                size="mini"
                <!-- :data="tableData" -->
                stripe
                style="width: 100%;height=600px">
                <el-table-column
                prop="date"
                label="日期2"
                width="180">
                </el-table-column>
                <el-table-column
                prop="name"
                label="姓名"
                width="180">
                </el-table-column>
                <el-table-column
                    prop="address"
                    label="地址">
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
      dialogExcelVisible:false,
      dialogBomHeaderVisible:false,
      dialogBomBodyVisible:false,
      selectingRow: null,
      tableData: [ ]
    }
  },
  components:{
  },
  mounted(){
    var me = this;
    
  },
  methods:{
     rowClick:function(row, column, event){
       console.log('rowClick');
        this.selectingRow = row;
     },
     currentChange:function(){
        //console.log('current-change');
     },

     addExcel:function(){
        //console.log('addExcel');
        //console.log(this);
        this.dialogExcelVisible = true;
        this.dialogBomHeaderVisible = false;

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
        me.dialogBomHeaderVisible = false;

         if(!me.selectingRow)
            return;
          
       
        console.log(me.selectingRow);
        me.selectingRowData = JSON.parse(JSON.stringify(me.selectingRow));
        //console.log(b);
        //me.formExcel = me.selectingRowData;
         me.deepClone(me.formExcel, me.selectingRowData);

        // me.formExcel.id = me.selectingRow.id;
        // me.formExcel.formatNo =me.selectingRow.formatNo;
        // me.formExcel.prdNoPos = me.selectingRow.prdNoPos;
        // me.formExcel.deptNoPos = me.selectingRow.deptNoPos;
        // me.formExcel.idNoPos = me.selectingRow.idNoPos;
        // me.formExcel.whFormType = me.selectingRow.whFormType;
        // me.formExcel.bomPlace = me.selectingRow.bomPlace
        // me.formExcel.startRow = me.selectingRow.startRow;
        // me.formExcel.bomStructCell = me.selectingRow.bomStructCell;
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
            // me.dialogExcelVisible = false;
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
        console.log(this.$refs["formExcel"]);
        this.$refs["formExcel"].resetFields();
        this.dialogExcelVisible = false;
     },
     handleEdit:function(index, row){
        
         this.dialogExcelVisible = false;
         this.dialogBomHeaderVisible = true;
         console.log(arguments);
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
  /* .MaintinExcelFormat_lyc /deep/ .el-input-number--mini {
      width: 60px;
      line-height: 26px;
  } */
 
</style>
