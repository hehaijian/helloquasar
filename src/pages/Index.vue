<template>
	<q-page class="flex flex-center">
		<img alt="Quasar logo" src="~assets/quasar-logo-full.svg">
		<div>
			电池状况: <strong>{{ batteryStatus }}</strong>
		</div>
		<div>
			<q-btn color="primary" label="Get Picture" @click="captureImage" />
			<div>
				<img :src="imageSrc" width="200">
			</div>
		</div>
	</q-page>
</template>

<script>
	export default {
		name: 'PageIndex',
		data() {
			return {
				batteryStatus: 'determining...',
				imageSrc: ''
			}
		},
		methods: {
			updateBatteryStatus(status) {
				console.log(status)
				this.batteryStatus = `电量: ${status.level}%, 充电状态: ${status.isPlugged}`
			},
			captureImage() {
				navigator.camera.getPicture(
					data => { // 如果成功
						console.log(data)
						this.imageSrc = `data:image/jpeg;base64,${data}`
					},
					() => { // 如果失败
						this.$q.notify('Could not access device camera.')
					}, {
						// 相机选项
						destinationType: Camera.DestinationType.DATA_URL //返回base64格式
					}
				)
			}
		},
		created() {
			// 我们注册事件, 参考插件的文档页面
			window.addEventListener('batterystatus', this.updateBatteryStatus, false)
		},
		beforeDestroy() {
			// 我们做一些清理工作;
			// 我们需要删除事件监听器
			window.removeEventListener('batterystatus', this.updateBatteryStatus, false)
		}
	}
</script>
