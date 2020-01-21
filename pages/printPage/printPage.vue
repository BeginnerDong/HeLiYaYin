<template>
	<view>
		<view><strong></strong>
			<image class="w bigImg" :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode="widthFix"></image>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.title = options.title;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		 onShow() {  
			const self = this;
			setTimeout(() => {  
				uni.setNavigationBarTitle({  
					title: self.title
				})  
			}, 500)  
		},
		
		methods: {
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: self.title
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.labelGet(postData, callback);
			},
		}
	};
</script>

<style>
	.bigImg{width: 100%;height: 100%;}
</style>
