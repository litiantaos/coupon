<template>
	<view v-if="isLoading" class="load-view" :style="{background}">
		<image class="icon" src="../../static/icons/loader.svg" mode="widthFix"></image>
	</view>
</template>

<script>
	export default {
		name: "load-view",
		props: {
			isLoading: {
				type: Boolean,
				default: false
			},
			background: {
				type: String,
				default: "#fff"
			}
		},
		data() {
			return {}
		}
	}
</script>

<style lang="scss" scoped>
	.load-view {
		position: fixed;
		top: 0;
		left: 0;
		z-index: 998;
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100vw;
		height: 100vh;
		pointer-events: none;
		transition: opacity .3s;

		.icon {
			width: 50rpx;
			animation: loading 1s linear infinite;

			@keyframes loading {
				0% {
					transform: rotate(0deg);
				}

				100% {
					transform: rotate(360deg);
				}
			}
		}
	}
</style>