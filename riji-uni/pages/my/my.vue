<template>
	<view class="my-wrap">
		<div class="getUserInfo">
			<div v-show="isShowUserInfo" class="userInfo">
				<div class="userInfo-image">
					<image :src="userInfo.avatarUrl" mode=""></image>
				</div>
				<h4>{{userInfo.nickName}}</h4>
			</div>
			<button bindgetuserinfo="bindGetUserInfo" @click="getUserInfoButton" v-show="!isShowUserInfo" class="getUserInfo-button">点击获取用户信息</button>
		</div>
		<div @click="toTeach" class="teach">
			<p>使用教程</p>
		</div>
		<div @click="toFeedback" class="feedback">
			<p>反馈</p>
		</div>
		<div @click="toAbout" class="about">
			<p>关于</p>
		</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isShowUserInfo: false,
				canIUse: wx.canIUse('button.open-type.getUserInfo'),
				userInfo: {}
			}
		},
		onLoad() {
			wx.getSetting({
			  success: (res) => {
			    if (!res.authSetting['scope.record']) {
			      wx.authorize({
			        scope: 'scope.record',
			        success: () => {
			          // 用户已经同意小程序使用录音功能，后续调用 wx.startRecord 接口不会弹窗询问
			          wx.startRecord()
			        }
			      })
			    }
			  }
			})
			wx.getSetting({
				success: (res) => {
					console.log(res, 'asd')
					if (res.authSetting['scope.userInfo']) {
						// 已经授权，可以直接调用 getUserInfo 获取头像昵称
						wx.getUserInfo({
							success: (res) => {
								this.userInfo = res.userInfo;
								this.isShowUserInfo = true;
								console.log(res.userInfo)
							}
						})
					} else {
						wx.authorize({
						  scope: 'scope.userInfo',
						  success: () => {
						    // 用户已经同意小程序使用录音功能，后续调用 wx.startRecord 接口不会弹窗询问
								wx.getUserInfo({
									success: (res) => {
										this.userInfo = res.userInfo;
										this.isShowUserInfo = true;
										console.log(res.userInfo)
									}
								})
						    wx.startRecord()
						  }
						})
					}
				}
			})
		},
		onReady () {
			
		},
		methods: {
			bindGetUserInfo (e) {
				console.log(e.detail.userInfo)
			},
			getUserInfoButton () {
				console.log('asd')
				wx.getSetting({
					success: (res) => {
						console.log(res, 'asd')
						if (res.authSetting['scope.userInfo']) {
							// 已经授权，可以直接调用 getUserInfo 获取头像昵称
							wx.getUserInfo({
								success: (res) => {
									this.userInfo = res.userInfo;
									this.isShowUserInfo = true;
									console.log(res.userInfo)
								}
							})
						}
					}
				})
			},
			toTeach () {
				uni.navigateTo({
					url: '../teach/teach'
				})
			},
			toFeedback () {
				uni.navigateTo({
					url: '../feedback/feedback'
				})
			},
			toAbout () {
				uni.navigateTo({
					url: '../aabout/aabout'
				})
			},
		}
	}
</script>

<style>
.my-wrap {
	width: 100%;
	height: 100%;
}
.getUserInfo {
	width: 100%;
	height: 250rpx;
	border-bottom: 2rpx solid #eee;
	display: flex;
}
.userInfo {
	width: 100%;
	display: flex;
	margin: 10rpx 0;
}
.userInfo h4 {
	margin: 75rpx 100rpx;
	line-height: 100rpx;
}
.userInfo-image {
	width: 200rpx;
	height: 200rpx;
	border-radius: 100%;
	margin:30rpx;
	overflow: hidden;
}
.userInfo-image image {
	width: 100%;
	height: 100%;
}
.getUserInfo-button {
	width: 300rpx;
	height: 80rpx;
	font-size: 30rpx;
	margin: auto;
}
.teach, .feedback, .about {
	width: 100%;
	height: 80rpx;
	line-height: 80rpx;
	font-size: 30rpx;
	padding: 10rpx 30rpx;
	margin: 10rpx 0;
	border-bottom: 2rpx solid #eee;
}
</style>
