<template>
	<view class="diary-list-wrap">
		<div v-if="isPoisList" class="diary-noList">
			<span>暂无日记</span>
			<button @click="toWrite">点击书写日记</button>
		</div>
		<div
			class="diary-list-item"
			v-for="item in poisList"
			:key="item"
			@click="toDetail(item.id)"
		>
			<div class="diary-list-item-left">
				<p>{{item.date.split('-')[0]}}</p>
				<p>{{item.date.split('-')[1]}}</p>
				<p>{{item.date.split('-')[2]}}</p>
			</div>
			<div class="diary-list-item-right">
				<image v-if="item.images.length !== 0" :src="item.images[0]" mode=""></image>
				<div v-if="item.images.length === 0" class="diary-list-item-right-text">
					<h6>{{item.title}}</h6>
					<p>{{item.content}}</p>
				</div>
			</div>
		</div>
	</view>
</template>

<script>
	export default {
		props: {
			
		},
		data() {
			return {
				poisList: [],
				isPoisList: false
			}
		},
		onShow () {
			this.poisList = uni.getStorageSync('diary_list');
			if (this.poisList.length == 0) {
				this.isPoisList = true;
			} else {
				this.isPoisList = false;
			}
		},
		methods: {
			toDetail (id) {
				uni.navigateTo({
					url: `../detail/detail?id=${id}`,
				})
			},
			toWrite () {
				uni.navigateTo({
					url: '../writeDiary/writeDiary'
				})
			}
		}
	}
</script>

<style>
.diary-list-wrap {
	width: 100%;
	height: 100%;
	overflow: hidden;
}
.diary-list-item {
	width: 80%;
	height: 200rpx;
	margin: 30rpx 10%;
	overflow: hidden;
	display: flex;
	border: 1rpx solid #EEEEEE;
	border-radius: 20rpx;
	box-shadow: #C0C0C0 10rpx 10rpx 10rpx;
}
.diary-list-item-left {
	width: 30%;
	text-align: center;
	border-right: 2rpx solid #C0C0C0;
}
.diary-list-item-left p {
	margin: 12rpx 0;
}
.diary-list-item-right {
	width: 70%;
}
.diary-list-item-right image {
	width: 100%;
	height: 100%;
}

.diary-list-item-right-text h6 {
	font-size: 42rpx;
	font-weight: 600;
	color: #000;
	padding: 20rpx;
}
.diary-list-item-right-text p {
	font-size: 24rpx;
	color: #C0C0C0;
	padding: 10rpx 20rpx;
}
.diary-noList {
	width: 100%;
	text-align: center;
}
.diary-noList span {
	color: #ccc;
	font-size: 30rpx;
	margin: 20rpx 0;
	position: absolute;
	top: 45%;
	left: 45%;
}
.diary-noList button {
	font-size: 26rpx;
	width: 60%;
	margin: 0 auto;
}
</style>
