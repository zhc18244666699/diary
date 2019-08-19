<template>
	<view class="detail-wrap">
		<div class="detail-swiper">
			<swiper class="swiper" :current="swiperCurrent" indicator-dots="true" :interval="interval" duration="500">
				<swiper-item v-for="item in diaryDetail.images" :key="item">
					<image :src="item" mode=""></image>
				</swiper-item>
			</swiper>
		</div>
		<div class="detail-title">
			<h5>{{diaryDetail.title}}</h5>
			<span>{{diaryDetail.date}}</span>
		</div>
		<div class="detail-content">
			<p>{{diaryDetail.content}}</p>
		</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				diaryDetail: {}
			}
		},
		onLoad (options) {
			const poisList = uni.getStorageSync('diary_list');
			poisList.map((item) => {
				if (item.id === Number(options.id)) {
					this.diaryDetail = item;
				}
			})
			console.log(this.diaryDetail)
		},
		methods: {
			
		}
	}
</script>

<style>
.detail-wrap {
	width: 100%;
	height: 100%;
	overflow: hidden;
}
.swiper {
	width: 100%;
	height: 400rpx;
	margin-bottom: 10rpx;
}
.detail-swiper {
	width: 100%;
}
.detail-swiper image{
	width: 100%;
}
.detail-title {
	width: 100%;
	height: 100rpx;
	text-align: center;
	margin: 10rpx 0;
}
.detail-title h5 {
	font-size: 42rpx;
	font-weight: 600;
	color: #000000;
}
.detail-title span {
	font-size: 30rpx;
	color: #CCCCCC;
}
.detail-content {
	width: 92%;
	margin: 10rpx 4%;
	letter-spacing: 2rpx;
	font-size: 26rpx;
	text-indent: 48rpx;
	line-height: 54rpx;
}
</style>
