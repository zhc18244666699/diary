<template>
	<view v-if="isShow" class="message-wrap">
		<p>{{showMessage}}</p>
	</view>
</template>

<script>
	export default {
		props: {
			showMessage: {
				type: String,
				default: '提示信息'
			},
			isShow: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {
				 
			}
		},
		onShow () {
			setTimeout(() => {
				this.$emit('hideMes', {})
			}, 2000)
		},
		methods: {
			
		}
	}
</script>

<style>
.message-wrap {
	width: 100%;
	height: 100%;
	overflow: hidden;
	position: absolute;
	top: 0;
	left: 0;
	transition: 1000ms;
}
.message-wrap p {
	width: 100%;
	height: 50rpx;
	background: #1296db;
	color: #fff;
	text-align: center;
	line-height: 50rpx;
	font-size: 26rpx;
	position: absolute;
	top: 0;
}
</style>
