<template>
    <div class="box">
        <div class="header">
           <el-row type="flex" >
                <el-col :span="6"><div class="grid-content bg-purple-dark"> PIC</div></el-col>
                <el-col :span="6" :offest="6"><div></div></el-col>
            </el-row>
        </div>
        <div class="contain">
            <el-tabs v-model="activeName" @tab-click="handleClick">
                <el-tab-pane label="Bom导入工具" name="first"  >
                    <ImportBom></ImportBom>
                </el-tab-pane>
                <el-tab-pane label="Bom模版" name="second"  >
                    <MantainExcelFormat></MantainExcelFormat>
                </el-tab-pane>
                <el-tab-pane label="智选Bom" name="free">智选Bom</el-tab-pane>
            </el-tabs>
        </div>
        <div class="footer">
            <el-row type="flex" justify="center" >
                <el-col :span="12"><div class="grid-content bg-purple-dark"> XXX股份有限公司 版权所有 Copyright | 2016 XX All Rights Reserved. 粤ICP备 1234567号-1</div></el-col>
            </el-row>
           </div>
    </div>

  
</template>
<script>
import ImportBom from './ImportBom.vue'
import MantainExcelFormat from './MantainExcelFormat.vue'

  export default {
    data() {
      return {
        activeName: 'second'
      };
    },
    components:{
        ImportBom,
        MantainExcelFormat
    },
    methods: {
      handleClick(tab, event) {
        console.log(tab, event);
      }
    }
  };
</script>

<style scoped>
.box{
    height: 100vh;
    display: flex;
    flex-direction: column;
    overflow: hidden;
}
.header{
    height: 40px;
    background: red;
}
.contain{
    display: flex;
    flex: 1;
    overflow-y: auto;
}
.footer{
    height: 30px;
    background: '#f9fafc';
    font-size: 12px
}
.mainbox{
    height: 2000px;
}

</style>>