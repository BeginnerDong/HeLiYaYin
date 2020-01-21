<template>
	<view>
		
		<view class="itemS fs13">
			<view class="item flexRowBetween">
				<view class="ll"><image class="L-icon" src="../../static/images/contact-icon.png" mode=""></image></view>
				<view class="rr flex">
					<view>接单电话：</view>
					<view class="pubColor mgl10 mgr10" @click="callPhone(mainData[0].description)">{{mainData[0].description}}</view>
					<view class="pubColor" @click="callPhone(mainData[0].url)">{{mainData[0].url}}</view>
				</view>
			</view>
			<view class="f5H5"></view>
			<view class="QQline">
				<view class="item flexRowBetween">
					<view class="ll"><image class="L-icon" src="../../static/images/contact-icon1.png" mode=""></image></view>
					<view class="rr flex borderB1">
						<view>QQ接单客服一：</view>
						<view class="pubColor mgl10 mgr10">{{mainData[1].description}}</view>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll"></view>
					<view class="rr flex">
						<view>QQ接单客服二：</view>
						<view class="pubColor mgl10 mgr10">{{mainData[1].url}}</view>
					</view>
				</view>
			</view>
			<view class="f5H5"></view>
			<view class="item flex">
				<view class="ll"><image class="L-icon" src="../../static/images/contact-icon2.png" mode=""></image></view>
				<view class="rr flex">厂址：{{mainData[2].description}}</view>
			</view>
			<view class="f5H5"></view>
			<view class="flexCenter">
				<view style="margin-top: 100rpx;" @click="ewmShow">
					<image class="ewmPic" :src="mainData[2]&&mainData[2].mainImg&&mainData[2].mainImg[0]?mainData[2].mainImg[0].url:''" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="ewmShow" v-show="is_ewmShow">
			<view class="cont">
				<image class="ewm" :src="mainData[2]&&mainData[2].mainImg&&mainData[2].mainImg[0]?mainData[2].mainImg[0].url:''" mode=""></image>
			</view>
			<view class="closeBtn"  @click="ewmShow">×</view>
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
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">关注我们</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/contactCompany/contactCompany'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">联系公司</view>
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
				mainData:[],
				is_show:false,
				is_ewmShow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			callPhone(phone){
				const self = this;
				uni.makePhoneCall({
				    phoneNumber: phone
				});
			},
			
			ewmShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_ewmShow = !self.is_ewmShow
			},
			
			getMainData() {
				const self = this;	
				const postData = {};	
				postData.searchItem = {
					thirdapp_id:2,	
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['联系公司']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					create_time:'asc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData',self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.labelGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	/* @import "../../assets/style/xieyiAlert.css"; */
	page{padding-bottom: 120rpx;}
	.itemS .item{padding: 0rpx 4%;}
	.itemS .item .ll{width: 10%;display: block;}
	.itemS .item .ll .L-icon{width: 30rpx; height: 30rpx;display: block;}
	.itemS .item .rr{width: 90%;padding: 30rpx 0;}
	.ewmPic{width: 257rpx; height: 257rpx;}
	
	.ewmShow{padding:80rpx 30rpx; position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 60;padding-bottom: 80rpx;}
	.ewmShow .cont{width: 300rpx; height: 300rpx;padding: 100rpx 80rpx;background: #fff;}
	.ewmShow .cont .ewm{width: 100%;height: 100%; display: block;}
	.ewmShow .closeBtn{top: auto;bottom: -30rpx;}
	
</style>
