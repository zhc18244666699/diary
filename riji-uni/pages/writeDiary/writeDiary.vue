<template>
	<view class="write-board">
		<div v-show="!isImageShow" class="write-upload-image">
			<div @click="uploadImage">
				<span>+</span>
			</div>
		</div>
		<div v-show="isImageShow" class="write-image">
			<swiper @change="swiperChange" class="swiper" :current="swiperCurrent" indicator-dots="true" :interval="interval" duration="500">
				<swiper-item v-for="item in imageList" :key="item">
					<image :src="item" mode=""></image>
				</swiper-item>
			</swiper>
			<div class="image-manage">
				<div @click="uploadImage" class="image-add">
					+
				</div>
				<div @click="reduceImage" class="image-reduce">
					-
				</div>
			</div>
		</div>
		<div class="write-title">
			<div class="write-title-text">
				<input v-model="title" class="write-title-text-input" maxlength="10" placeholder="标题" />
			</div>
			<div class="write-title-time">
				<picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
					<view class="uni-input">{{date}}</view>
				</picker>
			</div>
		</div>
		<div class="write-content">
			<textarea v-model="content" class="write-content-textarea" placeholder-style="color:#ccc" placeholder="请输入正文"/>
		</div>
		<div class="write-footer-save">
			<span @click="getAddress" class="write-footer-address">获取地理位置</span>
			<button class="write-footer-save-button" @click="save" >保存</button>
		</div>
		<MessageTop @hideMes="hideMessage" :isShow="isShowMessage" :showMessage="isMessage" />
	</view>
</template>

<script>
	import MessageTop from '../../components/messageTip/index.vue'
	export default {
		components: {
			MessageTop
		},
		data() {
			const currentDate = this.getDate({
					format: true
			})
			return {
				date: currentDate,
				title: '',
				content: '',
				isImageShow: false,
				imageList: [],
				swiperCurrent: 0,
				isShowMessage: false,
				isMessage: ''
			}
		},
		methods: {
			bindDateChange: function(e) {
				this.date = e.target.value
			},
			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();

				if (type === 'start') {
						year = year - 60;
				} else if (type === 'end') {
						year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			},
			uploadImage () {
				const imglist = this.imageList;
				uni.chooseImage({
				    count: 1,
				    sourceType: ['album'],
				    success: (res) => {
				        uni.getImageInfo({
				            src: res.tempFilePaths[0],
				            success: (imageMessage) => {
											const { path } = imageMessage
											imglist.push(path)
											this.imageList = imglist
											this.isImageShow = true;
				            }
				        });
				    }
				});
			},
			swiperChange(event) {
				const { detail } = event;
				const { current } = detail;
				this.swiperCurrent = current;
			},
			reduceImage () {
				if (this.imageList.length > 1) {
					const newImageList = this.imageList.splice(this.swiperCurrent, 1);
					this.swiperCurrent = this.swiperCurrent === 0 ? 0 : this.swiperCurrent - 1;
				} else {
					this.isImageShow = false;
				}
			},
			getAddress () {
				uni.navigateTo({
					url: '../address/address',
				})
			},
			hideMessage () {
				this.isShowMessage = false
			},
			save () {
				if (!this.title) {
					this.isShowMessage = true;
					this.isMessage = '请输入标题！';
					setTimeout(() => {
						this.isShowMessage = false;
					}, 2000)
				} else if (!this.content) {
					this.isShowMessage = true;
					this.isMessage = '请输入正文！';
				} else {
					const diaryMessage = {
						date: this.date,
						title: this.title,
						content: this.content,
						images: this.imageList,
						id: Math.round(Math.random() * 1000)
					}
					const storageDiaryList = uni.getStorageSync('diary_list');
					if (storageDiaryList) {
						storageDiaryList.push(diaryMessage)
						uni.setStorageSync('diary_list', storageDiaryList);
						uni.setStorage({
						    key: 'diary_list',
						    data: storageDiaryList,
						    success: function () {
									uni.switchTab({
										url: '../list/list'
									})
						    }
						});
					} else {
						const newDiaryList = [];
						newDiaryList.push(diaryMessage)
						uni.setStorageSync('diary_list', newDiaryList);
						uni.setStorage({
						    key: 'diary_list',
						    data: newDiaryList,
						    success: function () {
									uni.switchTab({
										url: '../list/list'
									})
						    }
						});
					}
				}
				
			}
		}
	}
</script>

<style>
.write-board {
	width: 100%;
	height: 100%;
	overflow: hidden;
	display: flex;
	flex-direction: column;
}
.write-upload-image {
	width: 100%;
	height: 500rpx;
	background: #f9f9f9;
	display: flex;
}
.write-image {
	width: 100%;
	height: 500rpx;
	position: relative;
}
.swiper {
	width: 100%;
	height: 100%;
}
.swiper image {
	width: 100%;
}

.image-manage {
	width: 50rpx;
	height: 130rpx;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	align-items: center;
	position: absolute;
	bottom: 100rpx;
	right: 30rpx;
}

.image-add, .image-reduce {
	width: 50rpx;
	height: 50rpx;
	line-height: 40rpx;
	text-align: center;
	font-size: 50rpx;
	color: #fff;
	border: 2rpx solid #FFF;
	border-radius: 100%;
}

.write-upload-image div{
	width: 200rpx;
	height: 200rpx;
	background: #ccc;
	margin: auto;
	font-size: 150rpx;
	line-height: 180rpx;
	text-align: center;
	color: #FFF;
}
.write-title {
	width: 100%;
	display: flex;
	height: 80rpx;
	line-height: 80rpx;
	margin: 20rpx 0;
}
.write-title-text {
	flex: 1;
}
.write-title-text-input {
	padding: 15rpx 10px;
}
.write-title-time {
	width: 40%;
	border: 2rpx solid #ccc;
	border-radius: 50rpx;
	margin: 0 20rpx;
	font-size: 30rpx;
	text-align: center;
}
.write-content {
	width: 100%;
	height: 400rpx;
}

.write-content-textarea {
	display: block;
	width: 100%;
	height: 100%;
	font-size: 28rpx;
	padding: 20rpx;
}

.write-footer-save {
	width: 100%;
	height: 120rpx;
	line-height: 150rpx;
	text-align: center;
	border-top: 2rpx solid #ccc;
	position: absolute;
	bottom: 0;
	left: 0;
	display: flex;
	justify-content: space-between;
	align-items: center;
}
.write-footer-address {
	margin: 0 10rpx;
}
.write-footer-save-button {
	width: 150rpx;
	height: 70%;
	border-radius: 50rpx;
	background: #1296db;
	margin: 0 30rpx;
	color: #FFF;
	font-size: 32rpx;
}
</style>
