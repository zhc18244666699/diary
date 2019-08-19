<template>
	<view class="about-wrap">
		<div class="about-erweima">
			<div>
				<image src="../../static/gh_32d1e463b3a4_258.jpg"></image>
			</div>
		</div>
		<div class="about-name">
			<h3>Record Good Time</h3>
		</div>
		<div class="about-version">
			<p>version: 1.0.0</p>
		</div>
		<div class="about-describe">
			<p>这是一款随心记的日记程序</p>
		</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
.about-wrap {
	width: 100%;
	height: 100%;
}
.about-erweima {
	width: 100%;
	height: 450rpx;
	display: flex;
	margin: 50rpx 0;
}
.about-erweima div {
	width: 50%;
	margin: auto;
}
.about-erweima div image {
	width: 100%;
}
.about-name, .about-version, .about-describe {
	width: 100%;
	margin: 40rpx 0;
	text-align: center;
}
</style>
