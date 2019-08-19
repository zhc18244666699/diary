<template>
	<view class="feedback-wrap">
		<div class="feedback-textarea">
			<textarea placeholder="请输入反馈的内容···" name="" id="" cols="30" rows="10">
			</textarea>
		</div>
		<div class="feedback-phone">
			<input placeholder="请输入您的手机号···" type="phone" value="" />
		</div>
		<div class="feedback-submit">
			<button @click="submit">提交反馈</button>
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
			return {
				isShowMessage: false,
				isMessage: '反馈成功'
			}
		},
		methods: {
			hideMessage () {
				this.isShowMessage = false
			},
			submit () {
				this.isShowMessage = true;
				setTimeout(() => {
					this.isShowMessage = false;
					uni.navigateBack()
				}, 500)
			}
		}
	}
</script>

<style>
.feedback-wrap {
	width: 100%;
	height: 100%;
}
.feedback-textarea {
	width: 100%;
	height: 400rpx;
}
.feedback-textarea textarea {
	width: 96%;
	height: 100%;
	border: 2rpx solid #ccc;
	margin: 4% 2%;
	font-size: 30rpx;
}
.feedback-phone {
	width: 96%;
	height: 80rpx;
	margin: 4% 2%;
	border: 2rpx solid #ccc;
	font-size: 30rpx;
	line-height: 120rpx;
}
.feedback-phone input {
	line-height: 120rpx;
}
.feedback-submit {
	margin: 4% 2%;
	width: 96%;
}
</style>
