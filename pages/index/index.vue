<template>
	<view>
		
		<view class="mglr4 pdtb15">
			<!-- banner -->
			<view class="index_topBj">
				<view class="banner-box">
					<view class="banner">
						<swiper class="swiper-box flex" indicator-dots="true" autoplay="true" interval="3000" duration="1000" >
							<block v-for="(item,index) in mainData.mainImg" :key="index">
								<swiper-item class="swiper-item">
									<image :src="item.url" class="slide-image"/>
								</swiper-item>
							</block>
						</swiper>
					</view>
				</view>
			</view>
			<!--  -->
			<view class="index_box1 flexRowBetween pdt20 pdb15  mgt25">
				<view class="Lbox">
					<view class="item flex radius10 mgb10" style="background: #ffead8;color: #ff8d2c;" 
					@click="Router.navigateTo({route:{path:'/pages/printPage/printPage?title=画册书刊'}})">
						<view><image class="icon mgr10" src="../../static/images/home-icon2.png" mode=""></image></view>
						<view>
							<view class="tit">画册书刊报价</view>
							<view class="goBtn">立即进入&gt;</view>
						</view>
					</view>
					<view class="item flex radius10"  style="background: #d6edff;color: #2ea3ff;" 
					@click="Router.navigateTo({route:{path:'/pages/printPage/printPage?title=合板单页'}})">
						<view><image class="icon mgr10" src="../../static/images/home-icon3.png" mode=""></image></view>
						<view>
							<view class="tit">合板单页保价</view>
							<view class="goBtn">立即进入&gt;</view>
						</view>
					</view>
				</view>
				<view class="Rbox radius10 flexColumn" style="justify-content: center;color: #ff3b85;"  
				@click="Router.navigateTo({route:{path:'/pages/printPage/printPage?title=其他'}})">
					<view class="flexCenter mgb15"><image class="icon" src="../../static/images/home-icon4.png" mode=""></image></view>
					<view class="flexColumn">
						<view class="tit">其他印刷品保价</view>
						<view class="goBtn">立即进入&gt;</view>
					</view>
				</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/aboutUs/aboutUs'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">关于我们</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/contactCompany/contactCompany'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">联系公司</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: '首页轮播'
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
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 110rpx;} 
	
	/* banner */
	.swiper-box{height: 340rpx;box-sizing: border-box; width: 100%;border-radius: 8rpx;}
	.swiper-box image {width: 100%;height: 100%;}
	
	.index_box1 .Lbox{width: 360rpx;height: 340rpx;}
	.index_box1 .Lbox .item{height: 160rpx;padding: 0 30rpx;}
	.index_box1 .Rbox{width: 312rpx;height: 340rpx;background: #ffe2ed;}
	.index_box1 .icon{width: 70rpx; height: 70rpx; display: block;}
	.index_box1 .tit{font-size: 26rpx; font-weight: bold;margin-bottom: 10rpx;}
	.index_box1 .goBtn{font-size: 20rpx;}
	
</style>
