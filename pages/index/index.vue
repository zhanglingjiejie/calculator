<template>
	<view class="index_main">
		<view class="navigation"></view>
		<view class="btn_box">
			<view v-for="(item,index) in btnData" :key="index" @click="goCalculator(item.url)" class="item_box">
				<image :src="item.icon" mode=""></image>
				<view class="btn_text">{{item.title}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				btnData: [
					{'id': 1, 'title': '苹果计算器', 'url': '../calculator/apple','icon': "../../static/icon/计算器.png"},
					{'id': 2, 'title': '招标代理计算器', 'url': '../calculator/bids','icon': "../../static/icon/招标代理.png"},
					// {'id': 2, 'title': '工程招标', 'url': '../calculator/bids','icon': "../../static/icon/招标费业务.png"},
					// {'id': 3, 'title': '货物招标', 'url': '../calculator/bids','icon': "../../static/icon/招标费业务.png"},
				],
			}
		},
		onLoad() {

		},
		methods: {
			goCalculator(curl){
				console.log(curl)
				uni.navigateTo({
					url:curl
				})
			}
		}
	}
</script>

<style lang="scss">
	.index_main {
		.navigation{
			width: 100%;
			height: 200rpx;
			background: linear-gradient(to bottom, #607d8b 20%, #ff99cc00 100%);
		}
		.btn_box{
			.item_box{
				width: 40%;
				// background: #565353;
				display: inline-block;
				margin: 20rpx 30rpx;
				border-radius: 30rpx;
				text-align: center;
				.btn_text{
					font-size: 15px;
					color: #67615e;
				}
				image{
					height: 80rpx;
					width: 80rpx;
					margin: 10rpx;
				}
			}
		}
	}
</style>
