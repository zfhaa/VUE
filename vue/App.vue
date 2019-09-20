<template>
  <!-- <div>i am a cmp{{ msg }}</div> -->
  <div>
    <h2>线性进度条 --- 百分比外显</h2>
    <d-progress :percentage="10" status="success" />
    <d-progress :percentage="50" />
    <d-progress :percentage="100" status="success" />
    <d-progress :percentage="50" status="exception" />

    <br />
    <h2>线性进度条 --- 百分比内显</h2>
    <d-progress :stroke-width="18" :percentage="0"  :text-inside="true" :show-text="false"/>
    <d-progress :stroke-width="18" :percentage="50"  :text-inside="true" />
    <d-progress :stroke-width="18" :percentage="100" status="success" :text-inside="true" />
    <d-progress :stroke-width="18" :percentage="50" status="exception" :text-inside="true" />


    <br />
    <h2>环形进度条</h2>
    <d-progress  :percentage="0"  type="circle"/>
    <d-progress  :percentage="80"  type="circle"/>
    <d-progress  :percentage="100" status="success" type="circle"/>
    <d-progress  :percentage="50" status="exception" type="circle"/>
  </div>
  
</template>

<script>
import DProgress from "./Progress"; //es6语法：从文件夹中引入名为DProgress的组件
export default {
  components: {
    //使用组件
    DProgress
  },
  data() {
    return {
      msg: "hello world"
    };
  }
};
</script>

<style>
</style>

