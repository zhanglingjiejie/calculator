<template>
	<view class="index_main">
		<view class="navigation">
			<view class="back_btn" @click="goBack()">
				<image src="../../static/icon/back.png" mode=""></image>
			</view>
		</view>
		<view class="ca_box">
			<view class="title">招标代理服务费计算</view>
			<!-- <view class="service">
				<p>服务介绍</p>
				<div class="service_content">
					<p>按照国家发展计划委员会文件《计价格〔2002〕1980号》</p>
					<p>发改办价格[2003]857号规定以及国家发展改革委关于降低部分建设项目收费标准规范收费行为等有关问题的通知（发改价格[2011]534号）</p>
					<p>中标人须交纳招标代理费（按国家发展计划委员会文件《计价格〔2002〕1980号》和《发改价格[2011]534号》的规定计算）</p>
					<p>依此标准，可进行上下20%的浮动。</p>
				</div>
			</view> -->
			<view class="">
				<view class="ca_compute_box">
					<view class="left_title">计算项目</view>
					<view class="right_btn" @click="open()">
					  <span>{{bidsText}}</span>
					  <image src="../../static/icon/下箭头.png" mode=""></image>
					  <!-- <img src="../home/images/drop_icon.png" alt=""> -->
					</view>
				</view>
				<view class="ca_compute_box">
					<view class="left_title">中标金额（万元）</view>
					<input class="right_input" type="number" name="money" placeholder='请输入中标金额(万)' v-model="money"/>
				</view>
				<view class="ca_compute_box">
					<view class="left_title">优惠比例（%)</view>
					<input class="right_input" type="number" name="ratio" placeholder='请输入优惠比例(%)' v-model="ratio"/>
				</view>
			</view>
		</view>
		<view class="ca_btn_box">
			<view class="item_btn" @click="calculation()">
				<span class="item_btn_text">计算</span>
			</view>
		</view>
		
		<view class="charge" v-if="showCharge">
			<span>标准收费：<input type="text" :value="total" style="width:100px;" disabled="disabled">元/次</span>
		</view>
		<view class="charge" v-if="showCharge">
			<span>优惠收费：<input type="text" :value="discount" style="width:100px;" disabled="disabled">元/次</span>
		</view>
		
		
		<uni-popup ref="popup" type="center" :mask-click="popue">
			<view class="popup_box">
				<view class="popup_compute">
					<view class="compute_single" v-for="(item,pindex) in bidsData" :key="pindex" @click="choice(item)">
						<view class="span">{{item.title}}</view>
					</view>
				</view>
				<view class="compute_close" @click="cancel">
					<view class="span">取消</view>
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				bidsData: [
					{'id': 1, 'title': '服务招标'},
					{'id': 2, 'title': '工程招标'},
					{'id': 3, 'title': '货物招标'},
				],//类目数据
				bid: '',//类目id
				bidsText: '请选择类目',//类目标题
				popue: true,
				money: '',
				ratio: '',
				showCharge: false,
				total: '', // 标准收费价格
				discount: '', // 对应的折扣价格
			}
		},
		onLoad() {
			this._getBids()

		},
		methods: {
			// 计算项目
			_getBids(item){
				var that = this;
				uni.request({
					method:'GET',
					success(res) {
						console.log(res.data.data)
						if (res.data.code === 200) {
							that.bidsData = res.data.data // 计算类目
							that.bid = that.bidsData.length > 0 ? that.bidsData[0].id : 1
							that.bidsText = that.bidsData.length > 0 ? that.bidsData[0].title : '请选择项目'
						}
					}
				})
			},
			calculation(){
				var that = this;
				if(!that.money){
					uni.showToast({
						title: "请填写金额",
						icon: "none"
					})
					return false;
				}
				if(!that.ratio){
					that.ratio = 100
				}
				uni.request({
					data:{
						bid:that.bid,
						money:that.money,
						ratio:that.ratio
					},
					method:'GET',
					success(res) {
						console.log(res.data.data)
						if (res.data.code === 200) {
							that.total = res.data.data.total;
							that.discount = res.data.data.ratio;
							that.showCharge = true;
						}
					}
				})
			},
			// 打开计算类目弹窗
			open(){
				this.$refs.popup.open()
			},
			// 关闭计算类目弹窗
			choice(item){
				console.log(item)
				this.bid = item.id;
				this.bidsText = item.title;
				this.$refs.popup.close()
			},
			// 关闭计算类目弹窗
			cancel(){
				this.$refs.popup.close()
			},
			goBack(){
				uni.navigateBack({delta:1})
			}
		}
	}
	
</script>

<style lang="scss">
	page{
		background: #fcfdfd;
	}
	.index_main{
		.navigation{
			width: 100%;
			height: 200rpx;
			background: linear-gradient(to bottom, #0072ff 20%, #ff99cc00 100%);
			.back_btn{
				position: absolute;
				top: 100rpx;
				left: 30rpx;
				image{
					width: 60rpx;
					height: 60rpx;
				}
			}
		}
		.ca_box{
			.title{
				color: #0072ff;
				text-align: center;
				font-weight: 800;
				padding: 40rpx 0rpx;
				font-family: fangsong;
				font-size: 22px;
			}
			.ca_compute_box{
				display: flex;
				margin: 20px 0px;
				padding: 5px 45px;
				.left_title{
					display: flex;
					align-items: center;
					width: 140px;
				}
				.right_btn{
					background: #87d5ff;
					text-align: center;
					border-radius: 15px;
					color: #ffffff;
					font-size: 15px;
					display: flex;
					align-items: center;
					justify-content: center;
					width: 150px;
					height: 30px;
					image{
						width: 50rpx;
						height: 50rpx;
						padding-left: 10px;
					}
				}
				.right_input{
					background: #dfe3e4b8;
					text-align: center;                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        
					
					border-radius: 15px;
					color: #5d5858;
					font-size: 15px;
					display: flex;
					align-items: center;
					justify-content: center;
					width: 150px;
					height: 30px;
				}
			}
		}
		.ca_btn_box{			
			display: flex;
			justify-content: center;
			.item_btn{
				width: 600rpx;
				height: 90rpx;
				text-align: center;
				line-height: 90rpx;
				border-radius: 25px;
				background: #b8d1ff;
				transform: scale(1.1);
				box-shadow: 0 6px 15px rgba(0,0,0.1,0.2);
				line-height: 90rpx;
				.item_btn_text{
					color: #0f81f7;
				}
			}
		
		
		}
		
		.charge{
			width: 100%;
			display: inline-flex;
			align-items: center;
			justify-content: space-between;
			font-size: 15px;
			color: #414141;
			margin-top: 20px;
			span{
				display: flex;
				margin-left: 45px;
			}
		}
		.popup_box{
			background-color: #FFF;
			border-radius: 20rpx;
			width: 300rpx;
			height: 400rpx;
			padding: 20px;
			.popup_compute{
				border-radius: 10rpx;
				margin-bottom: 20px;
				// background-color: #0077ff;
				// color: #e9f1e2;
				.compute_single{
					text-align: -webkit-center;
					.span{
						width: 200rpx;
						height: 50px;
						line-height: 50px;
						border-bottom: #00000021 solid 1px;
						border-radius: 10px;
						line-height: 55px;
					}
					
				}
			}
			.compute_close{
				.span{
					text-align: -webkit-center;
					line-height: 30px;
					border-radius: 10px;
					background-color: #2196f399;
				}
			}
		}
		
	}
</style>