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
  //查看器
  let viewer=new Cesium.Viewer("cesiumcontainer",{
    infoBox: false,   //隐藏信息框
    geocoder: true,   //搜索框
    homeButton: true, //home按钮
    sceneModePicker: false, //选择3d和其他模式
    baseLayerPicker: false, //是否显示图层选择
    navigationHelpButton: false, //帮助按钮
    animation: false,//是否播放动画功能
    timeline: false, //时间轴功能
    fullscreenButton:false, //是否显示全屏
    // 我使用高德影像地形地图
    imageryProvider: new Cesium.UrlTemplateImageryProvider({
    url: "https://webst02.is.autonavi.com/appmaptile?style=6&x={x}&y={y}&z={z}",
    }),
    terrainProvider: Cesium.createWorldTerrain(
      {
        requestVertexNormals:true,//光照
        requestWaterMask:true //水纹
      }
    ), //设置地形 
  }) 
  //隐藏logo
  viewer.cesiumWidget.creditContainer.style.display="none"
  // 再加上高德影像注记地图
  viewer.imageryLayers.addImageryProvider(
  new Cesium.UrlTemplateImageryProvider({
  url: "http://webst02.is.autonavi.com/appmaptile?x={x}&y={y}&z={z}&lang=zh_cn&size=1&scale=1&style=8",
  })
  );
  //生成position是某个地方的位置
  let position =Cesium.Cartesian3.fromDegrees(113.3191,23.109,2000)
  viewer.camera.flyTo({
    destination: position,
    orientation : {
        heading : Cesium.Math.toRadians(0),
        pitch : Cesium.Math.toRadians(-20),
        roll : 0.0
    }
  })

    //添加3D建筑
    const buildings=viewer.scene.primitives.add(
    new Cesium.createOsmBuildings()
  )
  //添加3D模型
  const airplane =viewer.entities.add({
    name: "Airplane",
    position: Cesium.Cartesian3.fromDegrees(113.3191,23.109,1500),
    // orientation : {
    //     heading : Cesium.Math.toRadians(0),
    //     pitch : Cesium.Math.toRadians(-90),
    //     roll : 0.0
    // },
    model:{
      uri:"./model/moon.glb",
      // minimumPixelSize:128  //设置最小像素
      // silhouetteSize:30, //设置轮廓
      // silhouetteColor:Cesium.Color.WHITE, //设置轮廓颜色
      distanceDisplayCondition: new Cesium.DistanceDisplayCondition(
        0,
        200000
      )//根据到相机的距离确定可见性
    }
  })
  console.log(airplane)
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
