<script>
	export default {
		globalData: {
			systemInfo: {},
			uniBaseUrl: 'https://mp-3430637f-7f9f-4db5-9362-7316f6003843.cdn.bspapp.com/coupon'
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