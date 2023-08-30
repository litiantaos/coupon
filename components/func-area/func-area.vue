<template>
	<view class="container">
		<view class="left" :style="{backgroundImage: `url(${imgUrl})`}" @click="toRandomFood">
			<view class="text-wrapper">
				<view class="title">今日吃啥好</view>
				<view class="subtitle">不知道吃什么 抽盲盒吧</view>
				<view class="button">GO</view>
			</view>
		</view>

		<view class="gap-y"></view>

		<view class="right">
			<view class="right-top">
				<view class="text-wrapper">
					<view class="title" style="color: #fff">看我家猫猫</view>
				</view>
				<view class="overlay"></view>
				<swiper class="swiper" autoplay circular>
					<swiper-item class="swiper-item" v-for="img in catImgs" :key="img">
						<image class="swiper-image" :src="img" mode="aspectFill"></image>
					</swiper-item>
				</swiper>
			</view>

			<view class="gap-x"></view>

			<view class="right-bottom">
				<view class="text-wrapper" style="color: #F9F8E8">
					<view class="title">干饭得有料</view>
					<view class="subtitle">敬请期待</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "func-area",
		data() {
			return {
				imgUrl: '',
				catImgs: []
			};
		},
		mounted() {
			let {
				uniBaseUrl
			} = getApp().globalData;

			this.imgUrl = uniBaseUrl + '/images/assets/food_poster.jpg';

			for (let i = 1; i <= 6; i++) {
				this.catImgs.push(uniBaseUrl + '/images/cat/' + i + '.jpg');
			}
		},
		methods: {
			toRandomFood() {
				uni.navigateTo({
					url: '/pages/random-food/random-food'
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.text-wrapper {
		color: darkred;
		position: absolute;
		top: 30rpx;
		left: 30rpx;
		z-index: 11;

		.title {
			font-weight: bold;
		}

		.subtitle {
			font-size: 24rpx;
			margin-top: 10rpx;
		}

		.button {
			font-size: 28rpx;
			font-weight: bold;
			line-height: 48rpx;
			padding: 0 25rpx;
			background: #f5f5f5;
			opacity: 0.9;
			border-radius: 24rpx;
			margin-top: 30rpx;
			width: fit-content;
		}
	}

	.overlay {
		width: 100%;
		height: 100%;
		background: linear-gradient(to right, rgba(233, 51, 35, .3), transparent);
		position: absolute;
		top: 0;
		left: 0;
		z-index: 10;
		pointer-events: none;
	}

	.container {
		display: flex;
		justify-content: space-between;
		height: 400rpx;

		.swiper {
			width: 100%;
			height: 100%;
			position: absolute;
			top: 0;
			left: 0;

			.swiper-item {
				width: 100%;
				height: 100%;

				.swiper-image {
					width: 100%;
					height: 100%;
					z-index: 9;
				}
			}
		}

		.left {
			flex: 1;
			height: 100%;
			background: #eee;
			border-radius: 20rpx;
			overflow: hidden;
			background-size: cover;
			background-position: center bottom;
			position: relative;
		}

		.gap-y {
			width: 30rpx;
		}

		.gap-x {
			height: 30rpx;
		}

		.right {
			flex: 1;
			display: flex;
			flex-direction: column;
			justify-content: space-between;

			.right-top,
			.right-bottom {
				background: #eee;
				border-radius: 20rpx;
				overflow: hidden;
				flex: 1;
				position: relative;
			}

			.right-bottom {
				background: #EC7357;
			}
		}
	}
</style>