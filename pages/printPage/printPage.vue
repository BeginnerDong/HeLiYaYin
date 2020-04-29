<template>
	<view>
		<view><strong></strong>
			<image v-for="(item,index) in mainData.mainImg" @click="previewImg(index)" class="w bigImg" :src="item.url" mode="widthFix"></image>
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
			
			previewImg(index) {
				const self = this;
				var urls = [];
				for (var i = 0; i < self.mainData.mainImg.length; i++) {
					urls.push(self.mainData.mainImg[i].url)
				};
				wx.previewImage({
				  current: index, // 当前显示图片的http链接
				  urls: urls // 需要预览的图片http链接列表
				})
			},
			
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
