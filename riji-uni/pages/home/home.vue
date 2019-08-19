<template>
	<view class="home-wrap">
		<div class="logo-image">
			<img src="http://img1.windmsn.com/b/1/112/11226/1122634.jpg" alt="">
			<div class="write-and-read">
				<div
					class="write-button"
					:style="[writeButton]"
					@click="writeWidthChange"
				>开始日记</div>
				<div class="read-button" @click="toDiaryList" v-show="readButton">回忆往昔</div>
			</div>
			<div class="show-time">
				<div class="time-month">{{dateTime[1]}}</div>
				<div class="time-week">{{dateTime[0]}}</div>
				<div class="time-date">{{date}}</div>
			</div>
		</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				dateTime: [],
				date: '',
				writeButton: {
					width: '65%'
				},
				readButton: true
			}
		},
		onShow () {
			const time = new Date().toString();
			this.date = this.getDate();
			this.dateTime = time.split(' ');
			this.writeButton.width = '65%';
			this.readButton = true;
		},
		methods: {
			getDate() {
				const date = new Date();
				const Y = date.getFullYear() + '年';
				const M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '月';
				const D = date.getDate() + '日 ';
				const h = date.getHours() + ':';
				const m = date.getMinutes() + ':';
				const s = date.getSeconds(); 
				return Y + M + D;
			},
			writeWidthChange () {
				this.readButton = false;
				this.writeButton.width = '100%';
				setTimeout(() => {
					uni.navigateTo({
					    url: '../writeDiary/writeDiary',
							animationType: 'pop-in',
					});
				}, 200)
			},
			toDiaryList () {
				uni.switchTab({
					url: '../list/list'
				})
			}
		}
	}
</script>

<style lang="scss">
.home-wrap {
	width: 100%;
	height: 100%;
}
.logo-image {
	width: 90%;
	margin: 10rpx auto;
	border-radius: 20rpx;
	position: relative;
}
.logo-image img {
	width: 100%;
	border-radius: 20rpx;
}
.write-and-read {
	width: 70%;
	height: 80rpx;
	position: absolute;
	bottom: 5%;
	left: 15%;
	background: #C0C0C0;
	border-radius: 50rpx;
	display: flex;
}
.read-button {
	width: 35%;
	height: 100%;
	border-radius: 50rpx;
	line-height: 80rpx;
	text-align: center;
	font-size: 24rpx;
	color: #FFF;
}
.write-button {
	height: 100%;
	background: #F1F1F1;
	color: #1296db;
	border-radius: 50rpx;
	line-height: 80rpx;
	text-align: center;
	font-size: 24rpx;
	transition: width 500ms;
}
.show-time {
	position: absolute;
	top: 10%;
	left: 10%;
	font-size: 46rpx;
	color: #000;
}
.show-time div {
	margin: 10rpx 0;
}
.time-month {
	font-size: 66rpx;
	margin: 20rpx;
	font-weight: 600;
}
</style>
