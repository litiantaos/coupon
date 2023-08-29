<template>
	<view class="what-eat">
		<view class="back" @click="getRandomFood">
			<view class="title-wrapper" :class="{'active': isRandom}">
				<view class="title">不知道吃什么？</view>
				<view class="subtitle">随机选一个吧！</view>
			</view>
		</view>
		<view class="right" :class="{'active': isRandom}" @click="handler">
			<view class="window">
				<view class="foods" :style="{transform: `translateY(${-offsetY}rpx)`, transitionDuration: `${duration}s`}">
					<view v-for="(food, index) in visibleFoods" :key="index" class="food">{{food.name}}</view>
				</view>
			</view>
			<image class="image" src="../../static/images/machine.png" mode="heightFix"></image>
		</view>
	</view>
</template>

<script>
	const db = uniCloud.database();

	export default {
		name: "what-eat",
		data() {
			return {
				foods: [],
				isRandom: false,
				visibleFoods: [],
				currentIndex: 0,
				defaultOffsetY: 60,
				offsetY: 0,
				defaultDuration: 0.05,
				duration: 0,
				startTime: 0,
				randomTime: 0,
				intervalId: null
			};
		},
		mounted() {
			this.getFoods();
		},
		methods: {
			handler() {
				this.isRandom = !this.isRandom;
			},
			getRandomFood() {
				uni.vibrateShort();
				this.isRandom = true;

				this.startTime = new Date().getTime();
				this.randomTime = Math.floor(Math.random() * 2000) + 2500;
				this.currentIndex = 0;

				this.intervalId = setInterval(() => {
					this.scrollFoods();
				}, 100);
			},
			async getFoods() {
				const res = await db.collection('foods').aggregate().sample({
					size: 100
				}).end();
				this.foods = res.result.data;
				this.visibleFoods = this.foods.slice(0, 3);
			},
			scrollFoods() {
				const currentTime = new Date().getTime();
				const elapsedTime = currentTime - this.startTime;

				this.duration = this.defaultDuration;
				this.offsetY = this.defaultOffsetY;
				this.currentIndex++;

				if (elapsedTime <= this.randomTime) {
					setTimeout(() => {
						this.duration = 0;
						this.offsetY = 0;
						this.visibleFoods.shift();

						if (this.currentIndex >= this.foods.length) this.currentIndex = 0;
						this.visibleFoods.push(this.foods[this.currentIndex]);
						// console.log(this.currentIndex, this.foods[this.currentIndex]);
					}, this.duration * 1000);
				} else {
					clearInterval(this.intervalId);
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.what-eat {
		height: 200rpx;
		position: relative;

		.back {
			width: 100%;
			height: 160rpx;
			background: #f8d5c8;
			border-radius: 20rpx;
			position: absolute;
			bottom: 0;
			display: flex;
			flex-direction: column;
			justify-content: center;
			box-sizing: border-box;
			transition: background .2s;

			&:active {
				background: #f8d5a9;
			}

			.title-wrapper {
				margin-left: 105rpx;
				width: fit-content;
				transform: translateX(0);
				transition: transform .5s;

				&.active {
					transform: translateX(-60rpx);
				}
			}

			.title,
			.subtitle {
				font-weight: bold;
			}

			.title {
				color: indianred;
				margin-bottom: 10rpx;
			}

			.subtitle {
				font-size: 40rpx;
				color: #E8423D;
			}
		}

		.right {
			position: absolute;
			top: 0;
			right: 50rpx;
			transform: scale(1) translate(0, 0);
			transition: transform .5s cubic-bezier(0.67, -0.06, 0.37, 1.41);
			;

			&.active {
				transform: scale(2) translate(-30rpx, 10rpx);
			}

			.window {
				width: 130rpx;
				height: 60rpx;
				// background: #fff;
				position: absolute;
				top: 60rpx;
				left: 22rpx;
				overflow: hidden;

				.foods {
					transition-property: transform;
					transition-timing-function: linear;

					.food {
						font-size: 22rpx;
						color: goldenrod;
						font-weight: bold;
						text-align: center;
						line-height: 60rpx;
					}
				}
			}

			.image {
				height: 210rpx;
			}
		}
	}
</style>