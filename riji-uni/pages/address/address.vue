<template>
	<view class="address-wrap">
		<div class="address-search">
			<input @input="searchInput" v-model="addressSearch" maxlength="20" placeholder="请输入您要搜索的地址" type="text">
			<button @click="searchAddress">确定</button>
		</div>
		<div class="address-map">
			<map
				id="myMap"
				style="width: 100%; height: 200px; display: flex;"
				:latitude="latitude"
				:longitude="longitude"
				:markers="markers"
				@regionchange="regionchange"
			>
			<image class="map-location" src="../../static/dingwei2.png" mode=""></image>
			</map>
		</div>
		<div class="address-list">
			<div class="address-list-item" v-for="(item, index) in POIList" :key="item">
				<div class="address-list-item-name">
					{{item.name}}
				</div>
				<span>
					{{item.name}}{{item.businessarea}}{{item.address}}
				</span>
			</div>
		</div>
	</view>
</template>

<script>
	var amapFile = require('../../utils/amap-wx.js')
	
	export default {
		data() {
			return {
				mapCtx: '',
				latitude: 39.909,
				longitude: 116.39742,
				markers: [{
					latitude: 39.909,
					longitude: 116.39742,
					iconPath: '../../static/dingwei.png',
					width: '60rpx',
					height: '65rpx'
				}],
				gaodeMap: '',
				POIList: [],
				addressSearch: '',
			}
		},
		onLoad () {
			this.mapCtx = uni.createMapContext('myMap');
			this.gaodeMap = new amapFile.AMapWX({key: 'f3bc57e09c404b724589dc91f70a0c49'});
			this.getPOIData()
		},
		methods: {
			regionchange (e) {
				this.mapCtx .getCenterLocation({
					success: (res) => {
						const { latitude, longitude } = res;
						this.latitude = latitude;
						this.longitude = longitude;
						this.markers[0].latitude = latitude;
						this.markers[0].longitude = longitude;
					}
				})
			},
			getPOIData () {
				this.gaodeMap.getRegeo({
					success: (data) => {
						//成功回调
						console.log(data[0].regeocodeData.pois[0])
						const { latitude, longitude, regeocodeData } = data[0];
						const poisList = regeocodeData.pois.splice(1, 15);
						this.latitude = latitude;
						this.longitude = longitude;
						this.markers[0].latitude = latitude;
						this.markers[0].longitude = longitude;
						this.POIList = poisList;
					},
					fail: function(info){
						//失败回调
						console.log(info)
					}
				})
			},
			searchInput () {
				console.log(this.addressSearch)
			},
			searchAddress () {
				
			}
		}
	}
</script>

<style>
.map-location {
	width: 60rpx;
	height: 66rpx;
	position: relative;
	left: calc(50% - 30rpx);
	top: calc(50% - 33rpx);
	overflow: hidden;
}
.address-search {
	width: 100%;
	height: 80rpx;
	display: flex;
	justify-content: space-between;
	align-items: center;
}
.address-search input {
	flex: 1;
	margin: 0 50rpx 0 20rpx;
	padding: 0 24rpx;
	font-size: 24rpx;
}
.address-search button {
	width: 120rpx;
	height: 60rpx;
	font-size: 24rpx;
	margin: 0 10rpx;
}
.address-list-item {
	width: 100%;
	display: flex;
	flex-direction: column;
	margin: 10rpx;
	border-bottom: 2rpx solid #eee;
}
.address-list-item span {
	font-size: 22rpx;
	color: #CCCCCC;
}
.address-list-item-name {
	width: 100%;
	font-size: 30rpx;
	color: #000;
}
</style>
