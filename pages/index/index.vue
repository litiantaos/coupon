<template>
	<nav-bar showTitle title="领券干饭小组"></nav-bar>

	<safe-area></safe-area>

	<view class="container">
		<view class="func-wrapper">
			<!-- <ad adpid=""></ad> -->
			<what-eat></what-eat>
		</view>

		<view class="tab" :style="{top: tabTop + 'px'}">
			<tab :items="tabItems" @event="getTabEvent"></tab>
		</view>

		<view class="tab-view">
			<view v-if="currentTabIndex == 0">
				<view v-for="event in events" :key="event._id">
					<view v-if="event.platform == 'meituan'" class="card" @click="getCoupon(event)">
						<image class="image" mode="widthFix" :src="event.poster_path" />
					</view>
				</view>
			</view>

			<view v-if="currentTabIndex == 1">
				<view v-for="event in events" :key="event._id">
					<view v-if="event.platform == 'eleme'" class="card" @click="getCoupon(event)">
						<image class="image" mode="widthFix" :src="event.poster_path" />
					</view>
				</view>
			</view>
		</view>

		<view style="height: 50rpx;"></view>
	</view>

	<popup ref="popup"></popup>
</template>

<script>
	const db = uniCloud.database();

	export default {
		data() {
			return {
				tabItems: [{
						img: '../../static/logos/meituan.png',
						text: '美团',
						check: true
					},
					{
						img: '../../static/logos/eleme.png',
						text: '饿了么',
						check: false
					}
				],
				currentTabIndex: 0,
				events: [],
				tabTop: 0
			}
		},
		onLoad() {
			this.getEvents();

			let {
				safeAreaInsets
			} = getApp().globalData.systemInfo;

			this.tabTop = safeAreaInsets.top + 44;
		},
		methods: {
			async getEvents() {
				const res = await db.collection('events').where('status != 0').orderBy('sort desc').get();
				this.events = res.result.data;
			},
			getCoupon(e) {
				if (e.app_id) {
					uni.navigateToMiniProgram({
						appId: e.app_id,
						path: e.navigate_path
					});
				} else if (e.token) {
					// console.log('show popup');
					this.$refs.popup.open(e.token);
				}
			},
			getTabEvent(e) {
				// console.log(e);
				this.currentTabIndex = e;
			}
		},
		onShareAppMessage() {
			return {
				title: "外卖领券领红包～干饭小组就位！",
				path: "/pages/index/index"
			}
		},
		onShareTimeline() {
			return {
				title: "外卖领券领红包～干饭小组就位！",
				query: "from=timeline"
			}
		}
	}
</script>

<style lang="scss">
	.container {
		// padding: 30rpx;

		.func-wrapper {
			margin: 30rpx;
		}
	}

	.tab {
		position: sticky;
		padding: 30rpx;
		background: #fff;
	}

	.tab-view {
		margin: 10rpx 30rpx 30rpx 30rpx;

		.card {
			border: 1rpx solid #eee;
			box-sizing: border-box;
			border-radius: 20rpx;
			overflow: hidden;
			margin-bottom: 30rpx;

			.image {
				width: 100%;
				vertical-align: bottom;
			}
		}
	}
</style>