<template>
	<view>
		<view class="uni-common-mt">
			<view class="input-list">
				<view class="input-text">手机号 : </view>
				<input focus placeholder="请输入手机号" />
			</view>
			<view class="input-list">
				<view class="input-text">验证码 : </view>
				<input placeholder="请输入验证码" />
				<text>获取</text>
			</view>
			<view class="input-list">
				<view class="input-text">密码 : </view>
				<input placeholder="请输入密码" />
			</view>
		</view>
		<view class="btn-bottom">
			<button type="primary">登录</button>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				
			}
		},
		onLoad() {
			
		},
		methods:{
			
		}
	}
	
	
</script>

<style lang="scss">
    @import '../../common/public.css';
	page{
		background-color: #faf7f7;
	}
	.uni-common-mt{
		width: 100%;
		margin-top: 10%;
		background-color: #ffffff;
		.input-list{
			display: flex;
			flex-direction: row;
			font-size: $uni-font-size-lg;
			padding: 0 6%;
			height: 120upx;
			line-height: 120upx;
			border-top: 1upx solid #cccccc;
			.input-text{
				color: $uni-text-color-black;
			}
			input{
				height: 120upx;
				line-height: 120upx;
				padding-left: 30upx;
			}
			text{
				color: #0e91ec;
				border: 2upx solid #0e91ec;
				box-sizing: border-box;
				height: 55upx;
				line-height: 55upx;
				padding: 0 15upx;
				border-radius: 20upx;
				margin-top: 35upx;
				margin-left: 70upx;
			}
		}
	}
</style>
