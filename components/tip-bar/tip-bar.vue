<template>
	<view class="container">
		<view class="title">温馨提示</view>
		<view class="content">
			<text>
				1. 红包领取有两种形式，一种是直接跳转对应平台小程序领取，另一种需要复制口令并手动打开对应平台App领取；\n
				2. 无论平台新老客户，外卖红包每个手机号每天均可领一次，红包金额随机发放；\n
				3. 平台绑定的手机号，需与领券登录的手机号一致。
			</text>
		</view>
	</view>
</template>

<script>
	export default {
		name: "tip-bar",
		data() {
			return {

			};
		}
	}
</script>

<style lang="scss" scoped>
	.container {
		padding: 30rpx;
		background: #f5f5f5;
		border-radius: 20rpx;

		.title {
			font-size: 26rpx;
			font-weight: bold;
			color: #333;
		}

		.content {
			font-size: 24rpx;
			color: #666;
			text-align: justify;
			line-height: 1.7;
			margin-top: 20rpx;
		}
	}
</style>