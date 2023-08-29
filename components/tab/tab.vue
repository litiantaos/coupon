<template>
	<view class="wrapper">
		<view v-for="(item, index) in newItems" :key="index">
			<view class="item" :class="{'active': item.check}" @click="onClick(index)">
				<img class="img" :src="item.img">
				<view class="text">{{item.text}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "tab",
		props: {
			items: {
				type: Array,
				default: []
			}
		},
		data() {
			return {
				newItems: []
			};
		},
		mounted() {
			this.newItems = this.items;
		},
		methods: {
			onClick(index) {
				// console.log(index);
				this.newItems[index].check = true;
				this.newItems.forEach((t, i) => {
					if (i != index) {
						t.check = false;
					}
				});
				this.$emit('event', index);
			}
		}
	}
</script>

<style lang="scss" scoped>
	.wrapper {
		width: 100%;
		display: flex;
		// justify-content: space-between;
		align-items: center;
		background: #fff;

		.item {
			display: flex;
			align-items: center;
			margin-right: 20rpx;
			padding: 15rpx;
			box-sizing: border-box;
			border-radius: 45rpx;

			&.active {
				background: #eee;
			}

			.img {
				width: 60rpx;
				height: 60rpx;
				border-radius: 50%;
				overflow: hidden;
			}

			.text {
				color: #333;
				font-size: 30rpx;
				font-weight: bold;
				margin-left: 20rpx;
				margin-right: 30rpx;
			}
		}
	}
</style>