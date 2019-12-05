<template>
	<view>
		<view class="myRowBetween fs13">
			<view class="item flexRowBetween">
				<view class="ll">成品尺寸</view>
				<view class="rr" @click="sizeShow">
					<view>大16开(210*285)</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">印刷颜色</view>
				<view class="rr" @click="colorShow">
					<view class="color9">请选择</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">封面P数</view>
				<view class="rr">4P</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">封面纸张</view>
				<view class="rr" @click="fmPaperShow">
					<view class="color9">请选择</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">内页P数</view>
				<view class="rr" @click="pPageShow">
					<view class="color9">请选择</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">内页纸张</view>
				<view class="rr"  @click="innerPageShow">
					<view class="color9">请选择</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">印刷数量</view>
				<view class="rr">
					<view class="input"><input type="text" value="0" /></view>本
				</view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll">封面工艺</view>
				<view class="rr" @click="Router.navigateTo({route:{path:'/pages/craft/craft'}})">
					<view class="color9">请选择</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">装订方式</view>
				<view class="rr" @click="bindingShow">
					<view class="color9">请选择</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="submitbtn mgt30 pdt15">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/picBookResult/picBookResult'}})">自动报价</button>
		</view>
		
		<!-- 印刷颜色 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="colorShow radius8 whiteBj" v-show="is_colorShow">
			<view class="tt" v-for="(item,index) in seltDate" @click="colorShow">{{item}}</view>
		</view>
		
		<!-- 成品尺寸弹框 -->
		<view class="qjAlertCont sizeShow" v-show="is_sizeShow">
			<view class="flexRowBetween pdtb15 mglr4 borderB1">
				<view class="fs13 color9" @click="sizeShow">取消</view>
				<view class="">成品尺寸</view>
				<view class="pubColor">确定</view>
			</view>
			<view class="infor fs13">
				<view class="item" v-for="(item,index) in sizeDate" @click="seltCoupon(index)">
					<view>{{item}}</view>
					<image class="setIcon" :src="seltCur == index?'../../static/images/confirm-icon3.png':''" alt="" />
				</view>
			</view>
		</view>
		
		<!-- 封面纸张弹框 -->
		<view class="qjAlertCont sizeShow" v-show="is_fmPaperShow">
			<view class="flexRowBetween pdtb15 mglr4 borderB1">
				<view class="fs13 color9" @click="fmPaperShow">取消</view>
				<view class="">封面纸张</view>
				<view class="pubColor">确定</view>
			</view>
			<view class="flexRowBetween twoCont">
				<view class="left classTit fs13">
					<view class="tt" :class="currTit==index?'on':''" v-for="(item,index) in fmPaperTit" @click="changeClassTit(index)">{{item}}</view>
				</view>
				<view class="right">
					<view class="infor fs13">
						<view class="item" v-for="(item,index) in fmPaperDate" :key="index" @click="seltFmPaper(index)">
							<view>{{item}}</view>
							<image class="setIcon" :src="fmPaperCur == index?'../../static/images/confirm-icon3.png':''" alt="" />
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 内页P数弹框 -->
		<view class="colorShow radius8 whiteBj" v-show="is_pPageShow">
			<view class="tt" v-for="(item,index) in pPageDate" :key="index"  @click="pPageShow">{{item}}</view>
		</view>
		
		<!-- 内页纸张弹框 -->
		<view class="qjAlertCont sizeShow" v-show="is_innerPageShow">
			<view class="flexRowBetween pdtb15 mglr4 borderB1">
				<view class="fs13 color9" @click="innerPageShow">取消</view>
				<view class="">内页纸张</view>
				<view class="pubColor">确定</view>
			</view>
			<view class="flexRowBetween twoCont">
				<view class="left classTit fs13">
					<view class="tt" :class="currTit==index?'on':''" v-for="(item,index) in fmPaperTit" @click="changeClassTit(index)">{{item}}</view>
				</view>
				<view class="right">
					<view class="infor fs13">
						<view class="item" v-for="(item,index) in fmPaperDate" @click="seltFmPaper(index)">
							<view>{{item}}</view>
							<image class="setIcon" :src="fmPaperCur == index?'../../static/images/confirm-icon3.png':''" alt="" />
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 装订方式弹框 -->
		<view class="colorShow radius8 whiteBj" v-show="is_bindingShow">
			<view class="tt"  v-for="(item,index) in bindingDate" :key="index"  @click="bindingShow">{{item}}</view>
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
				is_colorShow:false,
				seltDate:['单色','单色','四色'],
				is_sizeShow:false,
				seltCur:0,
				sizeDate:['大16开(210*285)','大8开(285*420)','大16开(185*260)','大8开(285*420)','大16开(210*285)','大8开(185*420)','大16开(185*260)'],
				is_fmPaperShow:false,
				currTit:0,
				fmPaperTit:['铜版纸','哑粉纸','白卡纸'],
				fmPaperCur:0,
				fmPaperDate:['105','128','157','200','250','300'],
				is_pPageShow:false,
				pPageDate:['20-40','40-60','60-80'],
				is_bindingShow:false,
				bindingDate:['骑马钉','无线胶装','有线胶装'],
				is_innerPageShow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			// 选择成品尺寸
			sizeShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_sizeShow = !self.is_sizeShow
			},
			// 选择印刷颜色
			colorShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_colorShow = !self.is_colorShow;
			},
			seltCoupon(index){
				const self = this;
				self.seltCur = index
			},
			// 选择封面纸张
			fmPaperShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_fmPaperShow = !self.is_fmPaperShow;
			},
			changeClassTit(index){
				const self = this;
				self.currTit = index
			},
			seltFmPaper(index){
				const self = this;
				self.fmPaperCur = index
			},
			// 内页P数
			pPageShow(index){
				const self = this;
				self.is_show = !self.is_show;
				self.is_pPageShow = !self.is_pPageShow
			},
			// 装订方式
			bindingShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_bindingShow = !self.is_bindingShow
			},
			// 内页纸张
			innerPageShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_innerPageShow = !self.is_innerPageShow
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
	@import "../../assets/style/editInfor.css";
	
	.myRowBetween .item .ll{width: 30%;}
	.myRowBetween .item .rr{width: 70%;}
	
	.myRowBetween .item .input{width: 100rpx;}
	.myRowBetween .item .input input{width: 100%;line-height: 40rpx;display: block;height: 40rpx; border-bottom: 1px solid #d9d9d9;text-align: center;}
	
	/* 选择成品尺寸弹框 */
	.sizeShow .infor{min-height: 240rpx;max-height:480rpx; overflow-y: auto;padding-bottom: 30px; }
	.sizeShow .item{padding:30rpx 4% 0 4%; display: flex; justify-content: space-between; align-items: center;}
	.sizeShow .item .setIcon{width: 30rpx; height: 24rpx; display: block;}
	
	/* 印刷颜色 */
	.colorShow{width: 80%; min-height: 240rpx;position: fixed; top: 50%;left: 50%; transform: translate(-50%,-50%);z-index: 90;}
	.colorShow .tt{padding: 15px 8%;border-bottom: 1px solid #eee;}
	.colorShow .tt:last-child{border-bottom: 0;}	
	
	.qjAlertCont{position: fixed; bottom: 0px;left: 0; width: 100%;background: #fff; z-index: 90;}
	
	.twoCont{align-items: flex-start;}
	.twoCont .left{width:30%;padding: 0 30rpx;box-sizing: border-box;}
	.classTit .tt{color: #666;margin-top:30rpx;}
	.classTit .tt.on{color: #222;}
	.twoCont .right{width: 70%;border-left: 1px solid #eee;box-sizing: border-box;padding-left:20rpx ;}
	
</style>
