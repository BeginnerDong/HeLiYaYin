<template>
	<view>
		<view class="myRowBetween fs13">
			<view class="item flexRowBetween">
				<view class="ll">成品尺寸</view>
				<view class="rr">大16开(210*285)</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">印刷颜色</view>
				<view class="rr">彩色</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">封面P数</view>
				<view class="rr">4P</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">封面纸张</view>
				<view class="rr">铜版纸 250克</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">内页P数</view>
				<view class="rr">16P</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">内页纸张</view>
				<view class="rr">铜板纸 157克</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">成品数量</view>
				<view class="rr">1000本</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">印后加工</view>
				<view class="rr">单面覆哑膜 烫黄金：20×30MM 骑马钉</view>
			</view>
		</view>
		<view class="pdlr4 f5bj flexCenter fs13 mgt15">
			<view class="pdtb15">印刷费用：<text class="price fs16 ftw">1850</text></view>
		</view>
		
		<view class="submitbtn mgt30 pdt15">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/picBook/picBook'}})">继续报价</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false
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
	}
</script>

<style>
	@import "../../assets/style/editInfor.css";
	
	.myRowBetween .item .ll{width: 30%;}
	.myRowBetween .item .rr{width: 70%;}

</style>
