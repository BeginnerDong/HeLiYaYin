<template>
	<view>
		<view class="myRowBetween fs13">
			<view class="item flexRowBetween">
				<view class="ll">成品尺寸</view>
				<view class="rr" @click="sizeShow">
					<view class="color9">{{sizeId>0?sizeDate[sizeIndex].title:'请选择'}}</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">印刷颜色</view>
				<view class="rr" @click="colorShow">
					<view class="color9">{{colorIndex>=0?colorData[colorIndex].title:'请选择'}}</view>
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
					<view class="color9">{{fmMaterialId>0&&fmSizeId>0?fmMaterialData[fmMaterialIndex].title+'-'+fmSizeData[fmSizeIndex].title:'请选择'}}</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">内页P数</view>
				<view class="rr" @click="pPageShow">
					<view class="color9">{{fmNumberIndex>=0?fmNumberData[fmNumberIndex].title:'请选择'}}</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">内页纸张</view>
				<view class="rr"  @click="innerPageShow">
					<view class="color9">{{isMaterialId>0&&isSizeId>0?isMaterialData[isMaterialIndex].title+'-'+isSizeData[isSizeIndex].title:'请选择'}}</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">印刷数量</view>
				<view class="rr">
					<view class="input"><input type="text" v-model="count"/></view>本
				</view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll">封面工艺</view>
				<view class="rr" @click="Router.navigateTo({route:{path:'/pages/craft/craft'}})">
					<view class="color9">{{fmgyData.length>0?'更改':'请选择'}}</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">装订方式</view>
				<view class="rr" @click="bindingShow">
					<view class="color9">{{bindingIndex>=0?bindingData[bindingIndex].title:'请选择'}}</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="submitbtn mgt30 pdt15">
			<button class="btn" type="button" @click="Utils.stopMultiClick(submit)">自动报价</button>
		</view>
		
		<!-- 印刷颜色 -->
		<view class="black-bj" v-if="is_show"></view>
		<view class="colorShow radius8 whiteBj" v-if="is_colorShow">
			<view class="tt" v-for="(item,index) in colorData" @click="colorChoose(index)">{{item.title}}</view>
		</view>
		
		<!-- 成品尺寸弹框 -->
		<view class="qjAlertCont sizeShow" v-if="is_sizeShow">
			<view class="flexRowBetween pdtb15 mglr4 borderB1">
				<view class="fs13 color9" @click="sizeShow">取消</view>
				<view class="">成品尺寸</view>
				<view class="pubColor" @click="sizeConfirm()">确定</view>
			</view>
			<view class="infor fs13">
				<view class="item" v-for="(item,index) in sizeDate" @click="sizeChoose(index)">
					<view>{{item.title}}</view>
					<image class="setIcon" v-if="sizeIndex == index" src='../../static/images/confirm-icon3.png' alt="" />
				</view>
			</view>
		</view>
		
		<!-- 封面纸张弹框 -->
		<view class="qjAlertCont sizeShow" v-if="is_fmPaperShow">
			<view class="flexRowBetween pdtb15 mglr4 borderB1">
				<view class="fs13 color9" @click="fmPaperShow">取消</view>
				<view class="">封面纸张</view>
				<view class="pubColor" @click="fmConfirm()">确定</view>
			</view>
			<view class="flexRowBetween twoCont">
				<view class="left classTit fs13">
					<view class="tt" :class="fmMaterialIndex==index?'on':''" v-for="(item,index) in fmMaterialData" 
					@click="fmMaterialChoose(index)">{{item.title}}</view>
				</view>
				<view class="right">
					<view class="infor fs13">
						<view class="item" v-for="(item,index) in fmSizeData" :key="index" @click="fmSizeChoose(index)">
							<view>{{item.title}}</view>
							<image class="setIcon" v-if="fmSizeIndex == index" src="../../static/images/confirm-icon3.png"  alt="" />
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 内页P数弹框 -->
		
		<view class="colorShow radius8 whiteBj" v-if="is_pPageShow">
			<view class="tt" v-for="(item,index) in fmNumberData" @click="fmNumberChoose(index)">{{item.title}}</view>
		</view>
		
		<!-- 内页纸张弹框 -->
		<view class="qjAlertCont sizeShow" v-if="is_innerPageShow">
			<view class="flexRowBetween pdtb15 mglr4 borderB1">
				<view class="fs13 color9" @click="innerPageShow">取消</view>
				<view class="">内页纸张</view>
				<view class="pubColor" @click="isConfirm()">确定</view>
			</view>
			<view class="flexRowBetween twoCont">
				<view class="left classTit fs13">
					<view class="tt" :class="isMaterialIndex==index?'on':''" v-for="(item,index) in isMaterialData" 
					@click="isMaterialChoose(index)">{{item.title}}</view>
				</view>
				<view class="right">
					<view class="infor fs13">
						<view class="item" v-for="(item,index) in isSizeData" @click="isSizeChoose(index)">
							<view>{{item.title}}</view>
							<image class="setIcon" v-if="isSizeIndex == index" src="../../static/images/confirm-icon3.png" alt="" />
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 装订方式弹框 -->
		<view class="colorShow radius8 whiteBj" v-if="is_bindingShow">
			<view class="tt"  v-for="(item,index) in bindingData" :key="index"  @click="bindingChoose(index)">
			{{item.title}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:[],
				is_show:false,
				is_colorShow:false,
				is_sizeShow:false,
				is_fmPaperShow:false,
				is_pPageShow:false,
				is_bindingShow:false,
				is_innerPageShow:false,
				sizeDate:[],
				sizeId:-1,
				sizeIndex:-1,
				colorData:[],
				colorId:-1,
				colorIndex:-1,
				fmMaterialData:[],
				fmMaterialIndex:-1,
				fmMaterialId:-1,
				fmSizeData:[],
				fmSizeIndex:-1,
				fmSizeId:-1,
				isMaterialData:[],
				isMaterialIndex:-1,
				isMaterialId:-1,
				isSizeData:[],
				isSizeIndex:-1,
				isSizeId:-1,
				fmNumberData:[],
				fmNumberIndex:-1,
				fmNumberId:-1,
				bindingData:[],
				bindingIndex:-1,
				bindingId:-1,
				count:'',
				fmId:-1,
				tjId:-1,
				jtId:-1,
				dmywId:-1,
				dmjbId:-1,
				fmgyData:[],
				resultData:[
					
				]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('data')){
				self.fmgyData = uni.getStorageSync('data');
				self.fmId = self.fmgyData[0].chooseId
				if(self.fmgyData[1].choose){
					self.tjId = self.fmgyData[1].hasId
				}else{
					self.tjId = self.fmgyData[1].noId
				};
				if(self.fmgyData[2].choose){
					self.jtId = self.fmgyData[2].hasId
				}else{
					self.jtId = self.fmgyData[2].noId
				};
				if(self.fmgyData[3].choose){
					self.dmywId = self.fmgyData[3].hasId
				}else{
					self.dmywId = self.fmgyData[3].noId
				};
				if(self.fmgyData[4].choose){
					self.dmjbId = self.fmgyData[4].hasId
				}else{
					self.dmjbId = self.fmgyData[4].noId
				};
				var text = self.fmgyData[0].title;
				for (var i = 1; i < self.fmgyData.length; i++) {
					if(self.fmgyData[i].choose){
						text += ' '+self.fmgyData[i].title
					}
				}
				self.handleResult(7,text)
				/* self.resultData.push(
					{index:7,text:text}
				); */
			}
		},
		
		methods: {
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				var sku_item = [self.sizeId,self.colorId,self.fmMaterialId,self.fmSizeId,self.isMaterialId,
				self.isSizeId,self.fmNumberId,self.bindingId,self.fmId,self.tjId,self.jtId,self.dmywId,self.dmjbId];
				var pass = true;
				for (var i = 0; i < sku_item.length; i++) {
					if(sku_item[i]<0){
						pass = false
					}
					for(var j=0;j<sku_item.length-i;j++){
					    //如果当前的元素(j)大于下一个元素(j+1)，则交换位置
				        if(sku_item[j]>sku_item[j+1]){
				            var t=sku_item[j];
				            sku_item[j]=sku_item[j+1];
				            sku_item[j+1]=t;
				        }
				    }
				};
				if (pass) {	
					/* self.resultData.push(
						{index:6,text:self.count}
					); */
					self.handleResult(6,self.count)
					self.resultData = self.$Utils.unique(self.resultData);
					const handle = (property) => {
						return function(a,b){
							const val1 = a[property];
							const val2 = b[property];
							return val1 - val2;
						}
					}
					
					self.resultData.sort(handle('index'));
					console.log('self.resultData',self.resultData);
					
					self.skuGet(sku_item);	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息，以便更准确的报价', 'none')
				};
				console.log(sku_item)
			},
			
			skuGet(sku_item) {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					sku_item:JSON.stringify(sku_item)
				};
				const callback = (res) => {				
					if (res.info.data.length > 0) {
						uni.setStorageSync('canClick', true);
						//uni.removeStorageSync('data');
						self.result = res.info.data[0];
						self.result.count = self.count;
						uni.setStorageSync('result',self.result);
						uni.setStorageSync('array',self.resultData);
						self.Router.navigateTo({route:{path:'/pages/picBookResult/picBookResult'}})
					} else {
						uni.setStorageSync('canClick', true);
						uni.showModal({
						    title: '提示',
						    content: '抱歉，您所选的规格下暂无报价',
							showCancel:false,
						    success: function (res) {
						        if (res.confirm) {
						             console.log('用户点击取消');
						        } else if (res.cancel) {
						            console.log('用户点击取消');
						        }
						    }
						});				
					}	
				};
				self.$apis.skuGet(postData, callback);
			},
			
			// 选择成品尺寸
			sizeShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_sizeShow = !self.is_sizeShow
			},
			
			sizeChoose(index){
				const self = this;
				self.sizeIndex = index;
			},
			
			handleResult(index,text){
				const self = this;
				if(self.resultData.length==0){
					self.resultData.push({index:index,text:text})
				}else{
					for (var i = 0; i < self.resultData.length; i++) {
						if(self.resultData[i].index==index){
							self.resultData[i].text = text;
						}else{
							self.resultData.push({index:index,text:text})
						}
					}
				}
			},
			
			
			sizeConfirm(){
				const self = this;
				
				self.sizeId = self.sizeDate[self.sizeIndex].id;
				self.handleResult(0,self.sizeDate[self.sizeIndex].title)
				//self.resultData.push({index:0,text:self.sizeDate[self.sizeIndex].title})
				
				self.is_sizeShow = false;
				self.is_show = false;
			},
			// 选择印刷颜色
			colorShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_colorShow = !self.is_colorShow;
			},
			
			colorChoose(index){
				const self = this;
				self.colorIndex = index;
				self.colorId = self.colorData[self.colorIndex].id;
				//self.resultData.push({index:1,text:self.colorData[self.colorIndex].title})
				//self.resultData.push({index:2,text:'4P'})
				self.handleResult(1,self.colorData[self.colorIndex].title);
				self.handleResult(2,'4P');
				self.is_colorShow = false;
				self.is_show = false;
			},
			
			// 选择封面纸张
			fmPaperShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_fmPaperShow = !self.is_fmPaperShow;
			},
			
			fmMaterialChoose(index){
				const self = this;
				self.fmMaterialIndex = index;
			},
			
			fmSizeChoose(index){
				const self = this;
				self.fmSizeIndex = index
			},
			
			fmConfirm(){
				const self = this;
				self.fmMaterialId = self.fmMaterialData[self.fmMaterialIndex].id;
				self.fmSizeId = self.fmSizeData[self.fmSizeIndex].id;
				/* self.resultData.push(
					{index:3,text:self.fmMaterialData[self.fmMaterialIndex].title+' '+self.fmSizeData[self.fmSizeIndex].title},
				); */
				self.handleResult(3,self.fmMaterialData[self.fmMaterialIndex].title+' '+self.fmSizeData[self.fmSizeIndex].title)
				self.is_show = false;
				self.is_fmPaperShow = false;
			},
			
			
			// 内页P数
			
			pPageShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_pPageShow = !self.is_pPageShow;
			},
			
			fmNumberChoose(index){
				const self = this;
				self.fmNumberIndex = index;
				self.fmNumberId = self.fmNumberData[self.fmNumberIndex].id;
				//self.resultData.push({index:4,text:self.fmNumberData[self.fmNumberIndex].title})
				self.handleResult(4,self.fmNumberData[self.fmNumberIndex].title)
				self.is_pPageShow = false;
				self.is_show = false;
			},
			// 装订方式
		
			bindingShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_bindingShow = !self.is_bindingShow;
			},
			
			bindingChoose(index){
				const self = this;
				self.bindingIndex = index;
				self.bindingId = self.bindingData[self.bindingIndex].id;
				/* self.resultData.push(
					{index:8,text:self.bindingData[self.bindingIndex].title}
				); */
				self.handleResult(8,self.bindingData[self.bindingIndex].title)
				self.is_bindingShow = false;
				self.is_show = false;
			},
			// 内页纸张
			innerPageShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_innerPageShow = !self.is_innerPageShow
			},
			
			isMaterialChoose(index){
				const self = this;
				self.isMaterialIndex = index;
			},
			
			isSizeChoose(index){
				const self = this;
				self.isSizeIndex = index
			},
			
			isConfirm(){
				const self = this;
				self.isMaterialId = self.isMaterialData[self.isMaterialIndex].id;
				self.isSizeId = self.isSizeData[self.isSizeIndex].id;
				/* self.resultData.push(
					{index:5,text:self.isMaterialData[self.isMaterialIndex].title+' '+self.isSizeData[self.fmSizeIndex].title},
				); */
				self.handleResult(5,self.isMaterialData[self.isMaterialIndex].title+' '+self.isSizeData[self.fmSizeIndex].title)
				self.is_show = false;
				self.is_innerPageShow = false;
			},
			
			getMainData() {
				const self = this;	
				const postData = {};	
				postData.searchItem = {
					thirdapp_id:2,	
					type:['in',[5,6]]
				};
				postData.order = {
					create_time:'asc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							if(self.mainData[i].title=='成品尺寸'){
								self.sizeDate = self.mainData[i].child
							};
							if(self.mainData[i].title=='印刷颜色'){
								self.colorData = self.mainData[i].child
							};
							if(self.mainData[i].title=='封面材质'){
								self.fmMaterialData = self.mainData[i].child
							};
							
							if(self.mainData[i].title=='封面尺寸'){
								self.fmSizeData = self.mainData[i].child
							};
							if(self.mainData[i].title=='内页材质'){
								self.isMaterialData = self.mainData[i].child
							};
							
							if(self.mainData[i].title=='内页尺寸'){
								self.isSizeData = self.mainData[i].child
							};
							if(self.mainData[i].title=='内页P数'){
								console.log(self.mainData[i])
								self.fmNumberData = self.mainData[i].child
							};
							if(self.mainData[i].title=='装订方式'){
								self.bindingData = self.mainData[i].child
							}
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
