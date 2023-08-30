<template>
	<view class="container">
		<view class="banner" :style="{backgroundImage: `url(${imgUrl})`}">
			<view class="title-wrapper" :class="{'active': isRandom}">
				<view class="title">不知道吃什么？</view>
				<view class="subtitle">
					<view class="subtitle-text">随机</view>
					<view class="subtitle-btn" @click="getRandomFood">选一个</view>
					<view class="subtitle-text">吧～</view>
				</view>
			</view>
		</view>

		<view class="machine-wrapper">
			<image class="machine" :src="machineImgUrl" mode="widthFix" :class="{'active': isRandom}">
				<view class="window">
					<view v-if="foods.length" class="food">{{foods[index].name}}</view>
				</view>
			</image>
		</view>
	</view>

	<load-view :isLoading="isLoading"></load-view>
</template>

<script>
	const db = uniCloud.database();

	export default {
		data() {
			return {
				foods: [],
				index: 0,
				startTime: 0,
				duration: 0,
				timer: null,
				isRandom: false,
				imgUrl: '',
				machineImgUrl: '',
				isLoading: true
			};
		},
		onLoad() {
			this.getFoods();

			let {
				uniBaseUrl
			} = getApp().globalData;

			this.imgUrl = uniBaseUrl + '/images/assets/fruit_tea.png';
			this.machineImgUrl = uniBaseUrl + '/images/assets/raffle_machine.png';
		},
		methods: {
			async getFoods() {
				const res = await db.collection('foods').aggregate().sample({
					size: 100
				}).end();

				this.foods = res.result.data;

				setTimeout(() => {
					this.isLoading = false;
				}, 500);
			},
			getRandomFood() {
				uni.vibrateShort();
				this.isRandom = true;

				this.startTime = new Date().getTime();
				this.duration = Math.floor(Math.random() * 4000) + 2500;

				setTimeout(() => {
					this.timer = setInterval(() => {
						this.scrollFoods();
					}, 100);
				}, 500);
			},
			scrollFoods() {
				const currentTime = new Date().getTime();
				const elapsedTime = currentTime - this.startTime;

				if (elapsedTime <= this.duration) {
					if (this.index < this.foods.length - 1) {
						this.index++;
					} else {
						this.index = 0;
					}
					// console.log(this.index, this.foods[this.index]);
				} else {
					clearInterval(this.timer);
					this.isRandom = false;
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.container {

		.banner {
			// height: 200rpx;
			background: #f8d5c8;
			background-size: auto 80%;
			background-position: right bottom;
			background-repeat: no-repeat;
			border-radius: 20rpx;
			display: flex;
			flex-direction: column;
			justify-content: center;
			box-sizing: border-box;
			padding-top: 40rpx;
			padding-bottom: 50rpx;
			padding-left: 40rpx;
			margin: 30rpx;
			// margin-bottom: 60rpx;

			.title-wrapper {
				width: fit-content;
			}

			.title {
				font-size: 42rpx;
				color: indianred;
				margin-bottom: 25rpx;
				font-weight: bold;
			}

			.subtitle {
				display: flex;
				align-items: center;

				.subtitle-text {
					font-size: 36rpx;
					color: indianred;
					font-weight: bold;
				}

				.subtitle-btn {
					font-size: 28rpx;
					color: #fff;
					padding: 10rpx 20rpx;
					background: #C4543B;
					border-radius: 15rpx;
					font-weight: bold;
					width: fit-content;
					margin: 0 15rpx;
					transition: background .2s;

					&:active {
						background: #9a412d;
					}
				}
			}
		}

		.machine-wrapper {
			margin: 30rpx;
			background: #eee;
			border-radius: 20rpx;
			padding: 50rpx 0;
			padding-left: 70rpx;
		}

		.machine {
			width: 600rpx;
			position: relative;

			&.active {
				animation: anima_tada 2s;
			}

			@keyframes anima_tada {
				0% {
					transform: scale(1);
				}

				5%,
				10% {
					transform: scale(.8) rotate(-5deg);
				}

				15%,
				25%,
				35%,
				45% {
					transform: scale(1.1) rotate(3deg);
				}

				20%,
				30%,
				40% {
					transform: scale(1.1) rotate(-3deg);
				}

				50%,
				100% {
					transform: scale(1);
				}
			}

			.window {
				position: absolute;
				top: 187rpx;
				left: 72rpx;
				width: 403rpx;
				height: 187rpx;
				// background: #fff;
				overflow: hidden;

				.food {
					font-size: 42rpx;
					color: goldenrod;
					font-weight: bold;
					text-align: center;
					line-height: 187rpx;
				}
			}
		}
	}
</style>