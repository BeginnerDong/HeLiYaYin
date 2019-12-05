<template>
	<view>
		
		<view class="seltCont fs13">
			<view class="item flexRowBetween" v-for="(item,index) in seltDate" :key="index">
				<view class="ll flex" @click="seltCurr(index)" >
					<view><image class="icon" :src="item.curr==true?'../../static/images/icon1.png':'../../static/images/icon2.png'" mode=""></image></view>
					<view>{{item.title}}</view>
				</view>
				<view class="rrSelt flexEnd" v-show="index==0" @click="seltShow">
					<view class="color9">请选择</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
		</view>
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/picBook/picBook'}})">确认选择</button>
		</view>
		
		<!-- 覆膜选择 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="seltShow radius8 whiteBj" v-show="is_seltShow">
			<view class="tt" @click="seltShow" v-for="(item,index) in seltList" :key="index">{{item}}</view>
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
				is_show:false,
				curr:0,
				seltDate:[
					{title:'覆膜',curr:false},
					{title:'烫金',curr:false},
					{title:'击凸',curr:false},
					{title:'单面压纹',curr:false},
					{title:'单面局部UV',curr:false}
				],
				seltList:['单面覆哑膜','单面付亮膜'],
				is_seltShow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			seltCurr(i){
				const self = this;
				self.seltDate[i].curr = !self.seltDate[i].curr
			},
			seltShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_seltShow = !self.is_seltShow;
			},
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
	
	.seltCont .item{padding: 30rpx 4%;border-bottom: 1px solid #eee;}
	.seltCont .item .ll{width: 60%;}
	.seltCont .item .rrSelt{width: 40%;}
	.seltCont .item .icon{width: 36rpx;height: 36rpx;margin-right: 20rpx;}

	
	/* 印刷颜色 */
	.seltShow{width: 80%; min-height: 204rpx;position: fixed; top: 50%;left: 50%; transform: translate(-50%,-50%);z-index: 90;}
	.seltShow .tt{padding: 15px 8%;border-bottom: 1px solid #eee;}
	.seltShow .tt:last-child{border-bottom: 0;}	
</style>
