<template>
	<view class="wrapper" :class="{'active': isShow}">
		<view class="overlay" :class="{'active': isShow}" @click="close"></view>
		<view class="main" :class="{'active': isShow}">
			<view class="content">{{props.content}}</view>
			<view class="button" @click="copyContent">{{buttonText}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "popup",
		data() {
			return {
				isShow: false,
				props: {},
				buttonText: '复制口令'
			};
		},
		methods: {
			open(e) {
				// console.log(e);
				this.isShow = true;
				this.props.content = e;
			},
			close() {
				this.isShow = false;
			},
			copyContent() {
				uni.setClipboardData({
					data: this.props.content,
					success: () => {
						this.buttonText = '已复制';
						setTimeout(() => {
							this.buttonText = '复制口令';
						}, 3000);
					}
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.wrapper {
		width: 100vw;
		height: 100vh;
		position: fixed;
		top: 0;
		left: 0;
		pointer-events: none;

		&.active {
			pointer-events: auto;
		}

		.overlay {
			width: 100%;
			height: 100%;
			position: fixed;
			top: 0;
			left: 0;
			background: #000;
			opacity: 0;
			transition: opacity .3s;

			&.active {
				opacity: .7;
			}
		}

		.main {
			width: 100%;
			height: 550rpx;
			background: #fff;
			position: fixed;
			bottom: 0;
			transform: translateY(100%);
			transition: transform .3s;
			box-sizing: border-box;
			border-radius: 30rpx 30rpx 0 0;
			padding: 40rpx 30rpx 0 30rpx;

			&.active {
				transform: translateY(0);
			}

			.content {
				background: #f5f5f5;
				border-radius: 20rpx;
				padding: 20rpx;
				font-size: 28rpx;
				color: #333;
				margin-bottom: 50rpx;
				height: 200rpx;
				display: flex;
				align-items: center;
				justify-content: center;
			}

			.button {
				background: #333;
				border-radius: 15rpx;
				font-size: 28rpx;
				color: #fff;
				width: 200rpx;
				text-align: center;
				line-height: 2.8;
				margin: 0 auto;
				transition: background .2s;

				&:active {
					background: #666;
				}
			}
		}
	}
</style>