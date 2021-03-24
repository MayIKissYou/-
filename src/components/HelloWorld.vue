<!--
 * @Description: 
 * @Version: 2.0
 * @Autor: Aaron
 * @Date: 2020-06-24 11:24:23
 * @LastEditors: Aaron
 * @LastEditTime: 2020-06-24 11:34:22
--> 
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      <a
        href="https://docs.cloudbase.net/cloudbase-vue/introduce.html"
        target="_blank"
        rel="noopener"
        >云开发 Vue 插件</a
      >
      文档
    </p>

    <template v-if="isLoginSuccss">
      <LoginState v-slot="{ loginState }">
        <p>{{ loginState ? "已登录" : "未登录" }}</p>
      </LoginState>
      <p>
        <a href="javascript:;" @click="callFunction">调用 hello world 云函数</a>
      </p>
    </template>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      isLoginSuccss: null,
      envId: "",
      callFunctionResult: "",
    };
  },
  props: {
    msg: String,
  },
  async created() {
    this.envId = this.$cloudbase.config.env;
    // 以匿名登录为例
    try {
      await this.$cloudbase
        .auth({ persistence: "local" })
        .anonymousAuthProvider()
        .signIn();

      this.isLoginSuccss = true;
    } catch (e) {
      if (e.message.includes("100007")) {
        this.isLoginSuccss = false;
      }
      console.error(e);
      console.log(e.code);
    }
  },
  methods: {
    async callFunction() {
      try {
        const res = await this.$cloudbase.callFunction({
          name: "hello_world",
          data: {
            foo: "bar",
          },
        });
        console.log(res)
        this.callFunctionResult = res;
      } catch (e) {
        this.callFunctionResult = e.message;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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

.hello {
  max-width: 500px;
  margin: 0 auto;
  word-break: break-all;
}
</style>
