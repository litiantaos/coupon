<script>
	export default {
		globalData: {
			systemInfo: {}
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
	/*每个页面公共css */
</style>