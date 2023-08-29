<template>
	<view class="nav-bar">
		<safe-area type="top"></safe-area>
		<view class="nav-bar_content" :style="{justifyContent}">
			<view v-if="showTitle" class="title">{{title}}</view>
			<slot></slot>
		</view>
	</view>
</template>

<script>
	export default {
		name: "nav-bar",
		props: {
			justifyContent: {
				type: String,
				default: "flex-start"
			},
			showTitle: {
				type: Boolean,
				default: false
			},
			title: {
				type: String,
				default: ""
			},
		},
		data() {
			return {};
		},
		methods: {}
	}
</script>

<style lang="scss" scoped>
	.nav-bar {
		position: fixed;
		top: 0;
		left: 0;
		z-index: 999;
		width: 100%;
		background: #fff;

		&_content {
			display: flex;
			align-items: center;
			height: 44px;
			padding: 6px 25rpx;
			box-sizing: border-box;

			.title {
				font-size: 36rpx;
				font-weight: bold;
			}
		}
	}
</style>