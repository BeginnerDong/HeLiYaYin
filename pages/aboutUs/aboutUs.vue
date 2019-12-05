<template>
	<view>
		
		<view class="mglr4 pdtb15">
			<view class="xqInfor fs12">
				<view>合力雅印印刷网由西安为你印电子商务有限公司联合陕西国泰防伪印务有限公司创立并运营，引进全新小森对开、四开印刷机，CTP出版系统及全套印后装订设备，拥有最先进的数字化印刷技术，数字化管理流程系统，借助“线上下单+线下工厂”的商业印刷新模式，专注画册印刷，为广告设计公司及印刷同行提供价格更低，交货更快、质量更好的网络印刷服务。</view>
				<view>为你印的自助报价系统，涵盖为你印网站、微信公众号、app，能让广告设计公司会员在几秒钟内准确计算出最权威、最合理的印刷品采购价格，让印刷采购报价完全自动化、透明化，让客户随时随地掌握印刷品采购成本及享受超低价格。</view>
				<view><image class="logo" src="../../static/images/about-icon.png" mode="widthFix"></image></view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/aboutUs/aboutUs'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">关注我们</view>
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
				showView: false,
				wx_info:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 120rpx;} 
	.xqInfor view{margin-bottom: 20rpx;}
	.xqInfor image{max-width: 100%;margin: 0 auto;display: block;}
	view .logo{width: 310rpx; height: 95rpx;margin: 160rpx auto;}
</style>
