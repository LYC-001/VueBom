<template>
  <el-container class="elmain_lyc">
    <el-main > 
        <el-row>
            <el-button  size="primary" >添加</el-button>
            <el-button  size="mini" type="primary">编辑</el-button>
            <el-button  size="mini" type="">删除</el-button>
            <el-button  size="mini" type="success">保存</el-button>
        </el-row>
            <el-table
                size="mini"
                :data="tableData"
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
                <el-table-column label="Bom"  width="280">
                    <template slot-scope="scope">
                        <el-button
                        size="mini" 
                        @click="handleEdit(scope.$index, scope.row)">表头</el-button>
                        <el-button
                        size="mini"
                        type="danger"
                        @click="handleDelete(scope.$index, scope.row)">表身</el-button>
                    </template>
                </el-table-column>
            </el-table>
    </el-main>
    <el-dialog title="Bom表头" :visible.sync="dialogBomHeaderVisible" > 
        <el-row>
            <el-button  size="primary">添加</el-button>
            <el-button  size="mini" type="">删除</el-button>
            <el-button  size="mini" type="success">保存</el-button>
        </el-row>
        <el-table
                size="mini"
                :data="tableData"
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
      dialogBomHeaderVisible:false,
      dialogBomBodyVisible:false,
      tableData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }]
    }
  },
  components:{
  },
  mounted(){
    var me = this;
    
  },
  methods:{
     handleEdit:function(index, row){
         this.dialogBomHeaderVisible = true;
         this.dialogBomBodyVisible = true;
        console.log(arguments);
     }
  }
}
</script> 


<style scoped>

</style>
