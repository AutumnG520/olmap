<template>
  <div class="hello">
    <div id="map">

    </div>
  </div>
</template>

<script>
import 'ol/ol.css';
import Map from 'ol/Map'; //init map
import View from 'ol/View'; //地图视图
import TileLayer from 'ol/layer/Tile'; //地图的图层
import { transform } from "ol/proj" //引入openlayer 坐标转换的
import OSM from 'ol/source/OSM'  //加载地图的
// import OverviewMap from 'ol/control/OverviewMap';//缩略图控件 
import ScaleLine from 'ol/control/ScaleLine';//比例尺控件
import Zoom from 'ol/control/Zoom';//添加缩放控件
import ZoomSlider from 'ol/control/ZoomSlider';//添加缩放滑动控件
import ZoomToExtent from 'ol/control/ZoomToExtent';//添加缩放到当前视图滑动控件
import FullScreen from 'ol/control/FullScreen';//添加全屏控件

export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  data(){
    return {
      map:null, //初始化地图1
      layers:[], //地图的layer
      view:null, //地图的view
    }
  },
  created () {
    
  },
  mounted(){
    this.mapInit(); //map初始化
  },
  methods:{
    // map初始化
    mapInit(){
       // 初始化layers
        this.layers.push(
            new TileLayer(
                {
                    source: new OSM(),
                    visible: true //指示该图层是否可见
                }
            )
        );
        // 初始化view
        this.view = new View({
            // 设置成都为地图中心，此处进行坐标转换， 把EPSG:4326的坐标，转换为EPSG:3857坐标，因为ol默认使用的是EPSG:3857坐标
            center: transform([104.06, 30.67], 'EPSG:4326', 'EPSG:3857'),
            zoom: 10
        })
      this.map = new Map(
        {   
            // logo: false, //设置地图logo不显示
            // logo: {src: '../img/face_monkey.png', href: 'http://www.openstreetmap.org/'},  //logo没测试
            layers: this.layers,
            view: this.view,
            target: 'map'
        }
      );
        // this.map.addControl(new OverviewMap({ //缩略图控件，好像并没有什么用
        //     collapsed: false 
        // }));
        this.map.addControl(new ScaleLine()); //比例尺控件
        //添加缩放控件
        this.map.addControl(new Zoom());
        //添加缩放滑动控件
        this.map.addControl(new ZoomSlider());
        //添加缩放到当前视图滑动控件
        this.map.addControl(new ZoomToExtent());
        //添加全屏控件
        this.map.addControl(new FullScreen());
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #map{
    width: 400px;
    height: 400px;
  }
</style>
<style>
/* 地图刚开始不显示，查看高度为0，然后设置这个高度（不知道为啥以后在说） */
  #map1 .ol-viewport{ 
    height: 400px;
  }
</style>
