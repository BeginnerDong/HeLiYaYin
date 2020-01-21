<template>
	<view>
		
		<view class="seltCont fs13">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="ll flex" @click="seltCurr(index)" >
					<view><image class="icon" :src="item.choose?'../../static/images/icon1.png':'../../static/images/icon2.png'" mode=""></image></view>
					<view>{{item.title}}</view>
				</view>
				<view class="rrSelt flexEnd" v-if="index==0" @click="seltShow">
					<view class="color9">{{fmIndex>=0?fmData[fmIndex].title:'请选择'}}</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
		</view>
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="btn" type="button" @click="submit"
			>确认选择</button>
		</view>
		
		<!-- 覆膜选择 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="seltShow radius8 whiteBj" v-show="is_seltShow">
			<view class="tt"  v-for="(item,index) in fmData" :key="index" @click="choose(index)">{{item.title}}</view>
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
				is_seltShow:false,
				fmData:[],
				tjData:[],
				tjId:-1,
				jtData:[],
				jtId:-1,
				dmywData:[],
				dmywId:-1,
				dmjbData:[],
				dmjbId:-1,
				mainData:[],
				fmId:-1,
				fmIndex:-1,
				
			}
		},
		
		onLoad(options) {
			const self = this;
			if(uni.getStorageSync('data')){
				self.mainData = uni.getStorageSync('data')
				self.fmData = self.mainData[0].child;
				self.fmId = self.mainData[0].chooseId;
				for (var i = 0; i < self.mainData[0].child.length; i++) {
					if(self.mainData[0].child[i].id == self.mainData[0].chooseId){
						console.log(2132)
						self.fmIndex = i
						console.log(self.fmIndex)
					}
				}
			}else{
				self.$Utils.loadAll(['getMainData'], self);
			}
			
		},
		
		methods: {
			
			
			submit(){
				const self = this;
				uni.setStorageSync('data',self.mainData);
				uni.navigateBack({
					delta:1
				})
			},
			
			
			seltCurr(index){
				const self = this;
				self.mainData = self.$Utils.cloneForm(self.mainData);
				self.mainData[index].choose = !self.mainData[index].choose;
				
				for (var i = 1; i < self.mainData.length; i++) {
					self.mainData[i].hasId = self.mainData[i].child[0].id
					self.mainData[i].noId = self.mainData[i].child[1].id
					
				}
				console.log(self.mainData)
			},
			
			seltShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_seltShow = !self.is_seltShow;
			},
			
			choose(index){
				const self = this;
				self.fmIndex = index;
				console.log(self.fmIndex)
				self.mainData[0].chooseId = self.fmData[self.fmIndex].id;
				self.fmId = self.fmData[self.fmIndex].id
				self.is_show = false;
				self.is_seltShow = false;
			},
			
			getMainData() {
				const self = this;	
				const postData = {};	
				postData.searchItem = {
					thirdapp_id:2,	
					type:['in',[5,6]],
					
				};
				postData.order = {
					create_time:'asc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						self.mainData.splice(0,8);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].choose = false;
							if(self.mainData[i].title=='覆膜'){
								self.mainData[i].choose = true;
								self.mainData[0].chooseId = 58;
								self.fmId = 58;
								self.fmIndex = 0;
								self.fmData = self.mainData[i].child
							};
							if(self.mainData[i].title=='烫金'){
								self.tjData = self.mainData[i].child
							};
							if(self.mainData[i].title=='击凸'){
								self.jtData = self.mainData[i].child
							};
							if(self.mainData[i].title=='单面压纹'){
								self.dmywData = self.mainData[i].child
							};
							if(self.mainData[i].title=='单面局部UV'){
								self.dmjbData = self.mainData[i].child
							};
						}
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
	
	.seltCont .item{padding: 30rpx 4%;border-bottom: 1px solid #eee;}
	.seltCont .item .ll{width: 60%;}
	.seltCont .item .rrSelt{width: 40%;}
	.seltCont .item .icon{width: 36rpx;height: 36rpx;margin-right: 20rpx;}

	
	/* 印刷颜色 */
	.seltShow{width: 80%; min-height: 204rpx;position: fixed; top: 50%;left: 50%; transform: translate(-50%,-50%);z-index: 90;}
	.seltShow .tt{padding: 15px 8%;border-bottom: 1px solid #eee;}
	.seltShow .tt:last-child{border-bottom: 0;}	
</style>
