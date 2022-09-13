<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  <div id="cesiumcontainer" ref="cesiumcontainer"></div>
</template>

<script setup>
import * as Cesium from "cesium"
// import HelloWorld from './components/HelloWorld.vue'
import { ref,reactive,onMounted } from 'vue';
import "./Widgets/widgets.css";
//设置静态资源根目录
window.CESIUM_BASE_URL ="/";
Cesium.Ion.defaultAccessToken='eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI4ZmVhNjRhNS04MGQ0LTRkYzMtOWVkMS1kZDVmY2U3MTEyYjkiLCJpZCI6MTA3MjkxLCJpYXQiOjE2NjI1MzgwNDl9.oPcSu5wH8UNKC5Q7r1tOw8e_g_60mh2AvbkFUg1GCJg';

// export default {
//   name: 'App',
//   components: {
//     HelloWorld
//   },
//   setup()
//   {

//   },
//设置默认视角为中国
Cesium.Camera.DEFAULT_VIEW_RECTANGLE=Cesium.Rectangle.fromDegrees(
  //西边的经度
  89.5,
  //南边纬度
  20.4,
  //东边精度
  110.4,
  //北纬
  61.2
)

onMounted(()=>{
  let viewer=new Cesium.Viewer("cesiumcontainer",{
    infoBox: false,   //隐藏信息框
  }) //查看器
  //隐藏logo
  viewer.cesiumWidget.creditContainer.style.display="none"
})

// }
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
#cesiumcontainer{
  width: 100vw;
  height: 100vh;

}
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
