<template>
	<view class="content">
		<view class="navigation">
			<view class="back_btn" @click="goBack()">
				<image src="../../static/icon/back.png" mode=""></image>
			</view>
		</view>
		<view class="result_view">
			<input class="result_box" type="text" v-model="showValue" disabled="true" :style="{fontSize: curFontSize}"/>
		</view>
		<view class="btns_view">
		    <view class="btn_radius" v-for="item in buttons" :key="item.text" :class="[item.class]"
		        @click="operate(item)">{{item.text}}</view>
		</view>
		<view class="btn_radius"></view>
	</view>
</template>

<script>
	// import {calc, fmt} from "a-calc";
	import { Decimal } from 'decimal.js';//引入
	export default {
		data() {
			return {
				firstValue: "",
				secondValue: "",
				operatorType: "",
				isCaculate: false,
				curFontSize: "180rpx",
				showValue: 0,
				buttons: [{
				        text: 'AC',
				        class: 'ft-color bg-gray ml-zero',
				        func: 'operator',
				        params: 'clear',
				    },
				    {
				        text: '+/-',
				        class: 'ft-color bg-gray',
				        func: 'operator',
				        params: 'opposite',
				    },
				    {
				        text: '%',
				        class: 'ft-color bg-gray',
				        func: 'operator',
				        params: 'percent',
				    },
				    {
				        text: '÷',
				        class: 'bg-orange',
				        func: 'operator',
				        params: 'divide',
				    },
				    {
				        text: '7',
				        class: 'bg-darkgray ml-zero',
				        func: 'inputText',
				        params: '7',
				    },
				    {
				        text: '8',
				        class: 'bg-darkgray',
				        func: 'inputText',
				        params: '8',
				    },
				    {
				        text: '9',
				        class: 'bg-darkgray',
				        func: 'inputText',
				        params: '9',
				    },
				    {
				        text: '×',
				        class: 'bg-orange',
				        func: 'operator',
				        params: 'multiply',
				    },
				    {
				        text: '4',
				        class: 'bg-darkgray ml-zero',
				        func: 'inputText',
				        params: '4',
				    },
				    {
				        text: '5',
				        class: 'bg-darkgray',
				        func: 'inputText',
				        params: '5',
				    },
				    {
				        text: '6',
				        class: 'bg-darkgray',
				        func: 'inputText',
				        params: '6',
				    },
				    {
				        text: '-',
				        class: 'bg-orange',
				        func: 'operator',
				        params: 'minus',
				    },
				    {
				        text: '1',
				        class: 'bg-darkgray ml-zero',
				        func: 'inputText',
				        params: '1',
				    },
				    {
				        text: '2',
				        class: ' bg-darkgray',
				        func: 'inputText',
				        params: '2',
				    },
				    {
				        text: '3',
				        class: ' bg-darkgray',
				        func: 'inputText',
				        params: '3',
				    },
				    {
				        text: '+',
				        class: 'bg-orange',
				        func: 'operator',
				        params: 'plus',
				    },
				    {
				        text: '0',
				        class: 'btn-size2 bg-darkgray ml-zero',
				        func: 'inputText',
				        params: '0',
				    },
				    {
				        text: '.',
				        class: 'bg-darkgray',
				        func: 'inputText',
				        params: '.',
				    },
				    {
				        text: '=',
				        class: 'bg-orange',
				        func: 'operator',
				        params: 'result',
				    }
				],
			}
		},
		onLoad() {
			
		},
		watch: {
		    showValue(newVal, oldVal) {
		        newVal += "";
				if(newVal.length>14){
					this.curFontSize = "80rpx";
				}else if(newVal.length>13){
					this.curFontSize = "90rpx";
				}else if(newVal.length>11){
					this.curFontSize = "100rpx";
				}else if(newVal.length>10){
		            this.curFontSize = "120rpx";
				}else if(newVal.length>9){
					this.curFontSize = "130rpx";
				}else if(newVal.length>8){
		            this.curFontSize = "150rpx";
				}else{
					this.curFontSize = "180rpx";
				}
		    }
		},
		methods: {
			operate(item) {
				console.log(item)
				const func = item.func;
				const params = item.params;
				switch (func) {
					case "operator":
						this.operator(params);
						break;
					case "inputText":
						this.inputText(params);
						break;
				}
			},
			caculate: function() {
				console.log(this.firstValue)
				var fval = Number(this.firstValue);
				var sval = Number(this.showValue);
				switch (this.operatorType) {
					case "plus":
						this.showValue = fval + sval;
						break;
					case "minus":
						this.showValue = fval - sval;
						break;
					case "multiply":
						this.showValue = fval * sval;
						break;
					case "divide":
						this.showValue = fval / sval;
						break;
					case "":
						break;
				}
			},
			operator(params) {
				console.log("计算操作");
				switch (params) {
					case "clear":
						// 清空
						this.showValue = "0";
						break;
					case "opposite":
						// 改变正负
						// 负负得正
						this.showValue = -this.showValue;
						break;
					case "percent":
						// 除以 100
						this.showValue = Number(this.showValue) / 100;
						break;
					case "result":
						// 除以 100
						this.caculate();
						break;
					default:
						this.operatorType = params;
						if (this.firstValue != "") {
						    this.caculate();
						}
						break;
				}
				this.isCaculate = true;
			},
			inputText(params) {
				console.log("输入操作");
				if (this.isCaculate) {
					this.firstValue = this.showValue;
					this.showValue = "0";
					this.isCaculate = false;
				}
				// 如果输入的是., 并且已经有., 那么什么都不做
				if (params == "." && this.showValue.indexOf(".") > -1) return;
				// 判断当前输入内容长度是否超过11, 超过11也什么都不做
				if (this.showValue.length >= 15) return;
				// 如果输入的不是., 并且当前没有数据, 那么就直接赋值
				if (params != "." && this.showValue == "0") {
					this.showValue = params + "";
				} else {
					// 如果是其它内容, 那么就追加
					this.showValue += params + "";
				}
				// 如果是整数, 还需要添加位数符号
				if (params != "." && this.showValue.indexOf(".") == -1) {
					let num = parseInt(this.showValue.replaceAll(",", ""));
					this.showValue = num.toLocaleString();
				}
			},
			goBack(){
				uni.navigateBack({delta:1})
			}
		}
	}
</script>

<style lang="scss">
	page {
		width: 100%;
		height: 100%;
		.navigation{
			width: 100%;
			height: 200rpx;
			// background: linear-gradient(to bottom, #0072ff 20%, #ff99cc00 100%);
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
		.content {
			width: 100%;
			height: 100%;
			background-color: black;
			padding-left: 30rpx;
			padding-right: 30rpx;
			box-sizing: border-box;
			.result_view {
			    width: 100%;
			    height: 35%;
			    position: relative;
			
			    .result_box {
			        height: 30%;
			        text-align: right;
			        color: white;
			        position: absolute;
			        
			        left: 0;
			        bottom: 0;
			    }
			}
			
			.btns_view {
			    width: 100%;
			    height: 65%;
			    display: flex;
			    flex-wrap: wrap;
			    justify-content: space-evenly;
			
			    .btn_radius {
			        margin-left: 30rpx;
			        width: 150rpx;
			        height: 150rpx;
			        line-height: 150rpx;
			        border-radius: 60%;
			        text-align: center;
			        font-size: 60rpx;
			        color: white;
					transition: background-color 0.5s ease; /* 背景色在0.3秒内平滑过渡 */
			    }
				.btn_radius:active {
				    transition: background-color 0.1s ease; /* 背景色在0.3秒内平滑过渡 */
				    background-color: #797979; /* 按钮按下时的颜色 */  
				}
				.ml-zero {
				    margin-left: 0;
				}
				.ft-color {
				    color: black;
				}
				.bg-orange {
				    background-color: orange;
				}
				.bg-darkgray {
				    background: #565656;
				}
				.bg-gray {
				    background-color: darkgrey;
				}
				.btn-size2 {
				    width: 326rpx;
				    border-radius: 75rpx;
				}
			}
		}
	}
</style>