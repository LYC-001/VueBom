<template>
<!-- <el-container>
  <el-header>
      <p>Header</p>
  </el-header>
  <el-main>
   
  </el-main>
   <el-footer>Footer</el-footer>


    
</el-container> -->
    <div class="box">
        <div class="header">
            <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
            <el-menu-item index="1">处理中心</el-menu-item>
            <el-submenu index="2">
                <template slot="title">我的工作台</template>
                <el-menu-item index="2-1">选项1</el-menu-item>
                <el-menu-item index="2-2">选项2</el-menu-item>
                <el-menu-item index="2-3">选项3</el-menu-item>
                <el-submenu index="2-4">
                <template slot="title">选项4</template>
                <el-menu-item index="2-4-1">选项1</el-menu-item>
                <el-menu-item index="2-4-2">选项2</el-menu-item>
                <el-menu-item index="2-4-3">选项3</el-menu-item>
                </el-submenu>
            </el-submenu>
            <el-menu-item index="3" disabled>消息中心</el-menu-item>
            <el-menu-item index="4"><a href="https://www.ele.me" target="_blank">订单管理</a></el-menu-item>
            </el-menu>

        </div>
        <div class="contain">

             
            <el-tabs v-model="activeName" @tab-click="handleClick">
                <el-tab-pane label="用户管理" name="first">
                    <HelloWorld></HelloWorld>
                </el-tab-pane>
                <el-tab-pane label="配置管理" name="second">配置管理</el-tab-pane>
                <el-tab-pane label="角色管理" name="third">角色管理</el-tab-pane>
                <el-tab-pane label="定时任务补偿" name="fourth">定时任务补偿</el-tab-pane>
            </el-tabs>
     
        </div>
        <div class="footer">footer</div>
    </div>

  
</template>
<script>
import HelloWorld from './HelloWorld.vue'
  export default {
    data() {
      return {
        activeName: 'second'
      };
    },
    components:{
        HelloWorld
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
    height: 60px;
    background: red;
}
.contain{
    display: flex;
    flex: 1;
    overflow-y: auto;
}
.footer{
    height: 60px;
    background: gold
}
.mainbox{
    height: 2000px;
}

</style>>