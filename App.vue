<script>
	export default {
		globalData: {
			systemInfo: {},
			uniBaseUrl: 'https://mp-f18043b0-ee24-4db2-b450-4b908c8b1bc2.cdn.bspapp.com/coupon'
		},
		onLaunch: function() {
			uni.getSystemInfo({
				success: res => {
					let menuButton = uni.getMenuButtonBoundingClientRect();

					let systemInfo = {
						screenWidth: res.screenWidth,
						screenHeight: res.screenHeight,
						statusBarHeight: res.statusBarHeight,
						safeAreaInsets: res.safeAreaInsets,
						menuButtonHeight: menuButton.height,
						menuButtonTop: menuButton.top,
						menuButtonBottom: menuButton.bottom
					};

					this.globalData.systemInfo = systemInfo;
				}
			});
		}
	}
</script>

<style>
	/* @import url('remixicon/fonts/remixicon.css'); */
</style>