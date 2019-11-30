<template>
  <div>
    <el-input v-model="input" placeholder="请输入内容"></el-input>
    <span>{{input}}</span>
  </div>
</template>

<script>
// import Top from './Top.vue'
export default {
  name: 'HelloWorld',
  data () {
    return {
      input: '',
      msg: 'Welcome to Your Vue.js App'
    }
  },
  components:{
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
</style>
