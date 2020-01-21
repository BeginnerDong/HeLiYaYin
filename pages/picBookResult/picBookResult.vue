<template>
	<view>
		<view class="myRowBetween fs13">
			<view class="item flexRowBetween">
				<view class="ll">成品尺寸</view>
				<view class="rr">{{array[0].text}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">印刷颜色</view>
				<view class="rr">{{array[1].text}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">封面P数</view>
				<view class="rr">{{array[2].text}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">封面纸张</view>
				<view class="rr">{{array[3].text}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">内页P数</view>
				<view class="rr">{{array[4].text}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">内页纸张</view>
				<view class="rr">{{array[5].text}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">成品数量</view>
				<view class="rr">{{array[6].text}}本</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">装订方式</view>
				<view class="rr">{{array[8].text}}本</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">印后加工</view>
				<view class="rr">{{array[7].text}}</view>
			</view>
		</view>
		<view class="pdlr4 f5bj flexCenter fs13 mgt15">
			<view class="pdtb15">印刷费用：<text class="price fs16 ftw">{{money}}</text></view>
		</view>
		
		<view class="submitbtn mgt30 pdt15">
			<button class="btn" type="button" @click="Router.redirectTo({route:{path:'/pages/picBook/picBook'}})">继续报价</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				result:{},
				is_show:false,
				array:[],
				money:0
			}
		},
		
		onLoad(options) {
			const self = this;
			if(uni.getStorageSync('array')){
				self.array = uni.getStorageSync('array')
			};
			if(uni.getStorageSync('result')){
				self.result = uni.getStorageSync('result');
				self.money = parseFloat(self.array[6].text)*parseFloat(self.result.price)
			};
			//self.$Utils.loadAll(['getMainData'], self);
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
