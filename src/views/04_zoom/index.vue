<template>
  <div class="vm">
    <h2 class="h-title">缩放</h2>

    <div id="map" class="map-x"></div>

    <div class="zoom">
      <p>zoom: {{zoom}}</p>
      <p>minZoom: {{minZoom}}</p>
      <p>maxZoom: {{maxZoom}}</p>
      <button @click="zoomIn">放大</button>
      <button @click="zoomOut">缩小</button>
    </div>

    <div class="explain">
      <p>地图分辨率：</p>
      <p>1、地图图像1像素宽度与实地距离的比值</p>
      <p>2、体现的是视图坐标系与地理坐标系之间的关系</p>
      <p>对弈每一个分辨率，有一个缩放等级：</p>
      <p>1、Openlayers最大支持29级分辨率</p>
      <p>2、不是每种地图都有29级分辨率，一般最精细到20级(google、OSM)</p>

      <hr>

      <p>map.getView()：获取当前视图</p>
      <p>map.getView().getZoom()：获取当前缩放级别</p>
      <p>map.getView().setZoom(...)：设置缩放级别</p>

    </div>
  </div>
</template>

<script>
import 'ol/ol.css'
import { Map, View } from 'ol'
import Tile from 'ol/layer/Tile'
import { OSM } from 'ol/source'

export default {
  name: 'Zoom',
  data () {
    return {
      map: null,
      zoom: 12,
      minZoom: 10,
      maxZoom: 14
    }
  },
  methods: {
    initMap () {
      this.map = new Map({
        target: "map",

        layers: [
          new Tile({
            source: new OSM()
          })
        ],
        
        view: new View({
          projection: "EPSG:4326",
          center: [116.404177,39.909652],
          zoom: this.zoom,        //打开页面默认的缩放级别
          minZoom: this.minZoom,  //地图缩放最小级别
          maxZoom: this.maxZoom   //地图缩放最大级别
        })
      })
    },
    zoomIn () {
      let view = this.map.getView()     //gitView()获取地图的View视图属性
      let zoom = view.getZoom()         //获取当前的缩放等级
      view.setZoom(zoom + 1)            //对地图的VIew对象中的属性进行修改,使其Zoom缩放等级增大
    },
    zoomOut () {
      let view = this.map.getView()
      let zoom = view.getZoom()
      view.setZoom(zoom - 1)
    }
  },
  mounted () {
    this.initMap()
  }
}
</script>

<style scoped>
  
    .zoom {
      display: flex;
      justify-content: center;
      margin: 10px 0 20px;
    }
    p {
      margin: 0 20px;
    }
    button {
      margin: 0 5px;
      background: #666;
      border: none;
      color: #fff;
      padding: 4px 12px;
      border-radius: 4px;
      outline: none;
      cursor: pointer;
    }
</style>