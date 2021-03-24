<!--
 * @Description: 
 * @Version: 2.0
 * @Autor: Aaron
 * @Date: 2020-06-24 11:24:23
 * @LastEditors: Aaron
 * @LastEditTime: 2020-06-24 12:58:01
--> 
<template>
  <div id="app">

    <HelloWorld msg="欢迎使用云开发" />
    <input type="file" ref="file"><button @click="uploadFile">图片上传</button>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data(){
    return {
      file:null
    }
  },
  methods:{
    async uploadFile(){
      this.file = this.$refs.file.files[0];
      this.$cloudbase.uploadFile({
          cloudPath: this.file.name, // 上传至云端的路径
          filePath: this.file, 
      }).then(res=>{
        console.log(res)
      })
      
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.logo {
  width: 120px;
  padding: 10px;
}
</style>
