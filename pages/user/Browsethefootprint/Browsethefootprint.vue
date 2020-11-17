
<template>
 
	<view class="map">
		<baidu-map class="map-contain" :scroll-wheel-zoom="true" :center="center" :zoom="zoom" MapType="BMAP_SATELLITE_MAP"
		 @ready="mapReady">
			<bm-geolocation anchor="BMAP_ANCHOR_BOTTOM_RIGHT" @locationSuccess="getMyLocation()" :showAddressBar="true"
			 :autoLocation="true"></bm-geolocation>
			<bm-marker @dragend="markerDrag" :position="center" :dragging="true" animation="BMAP_ANIMATION_BOUNCE">
				<!--<bm-label content="我爱北京天安门" :labelStyle="{color: 'red', fontSize : '24px'}" :offset="{width: -35, height: 30}"/> -->
			</bm-marker>
		</baidu-map>
	</view>
 
</template>
<script>
	import fengrui from "../../../component/test.js"
	export default {
		onLoad(res) {
			console.log(fengrui) 
			// console.log("加载百度地图")
			// var util = require('http://api.map.baidu.com/getscript?v=2.0&ak=v3Cg0hRvbX7cuUSKG2sEklyeP8iw5eye&services=&t=20200824135534');  //require这个js模块  
			// console.log(util)
 
		},
		data() {
			return {
				zoom: 18, //地图相关设置
				center: {
					lng: 0,
					lat: 0
				}
			}
		},
		methods: {
			markerDrag(x) {
				console.log(x.point);
				this.center.lat = x.point.lat;
				this.center.lng = x.point.lng;
			},
			mapReady({
				BMap,
				map
			}) {
				// 下面注释是百度地图API官方实现方法
				// console.log(1)
				// console.log(map);
				// console.log(BMap);
				let that = this;
				var geolocation = new BMap.Geolocation();
				// 开启SDK辅助定位
				geolocation.enableSDKLocation();
				geolocation.getCurrentPosition(function(r) {
					console.log("d");
					console.log(r);
					//getStatus拿到的是状态信息，失败与否
					if (this.getStatus() == BMAP_STATUS_SUCCESS) {
						// var mk = new BMap.Marker(r.point);
						// map.addOverlay(mk);//将覆盖物添加到地图中
						// map.panTo(r.point);//将地图的中心点更改为给定的点
						that.center.lng = r.point.lng;
						that.center.lat = r.point.lat;
						console.log('您所在位置为经度：' + r.point.lng + ',纬度：' + r.point.lat);
						// alert('您的位置：' + r.point.lng + ',' + r.point.lat);
					} else {
						console.log("位置信息获取失败，" + this.getStatus());
					}
				}, {
					enableHighAccuracy: true //要求浏览器获取最佳结果
				})
			},
 
		},
 
	}
</script>
 
<style>
template{
	width: 100%;
	height: 100%;
}
.map{
	width: 100%;
	height: 600px;
}
.map-contain{
	width: 100%;
	height: 100%;
}
</style>