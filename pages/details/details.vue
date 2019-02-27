
<template>
	<view>
		<!-- 头部 -->
		<view class="header">
			<uni-icon class="icon-left" type="arrowleft" size="30" @click="goDetail()"></uni-icon>
			<text class="title">{{title}}</text>
			
		</view>

		<view>
			
			
		</view>
	</view>
</template>

<script>
	import uniIcon from "../../components/uni-icon.vue";
	export default {
		components: {uniIcon},
		data(){
			return{
				title:""
			}
		},
		onLoad(e) {
			console.log(e.id);
			uni.request({
				url:'../../static/data/news.json',
				success: function(res) { 
					this.title = res.data.data[0].title;
					console.log(res.data.data[0].title);
				}
			});
		},
		methods:{
			goDetail: function(){
				uni.switchTab({
					url:'../news/news'
				})
			}
		}
		
	}
</script>

<style lang="scss">
	.header{
		width: 100%;
		height: 88upx;
		line-height: 88upx;
		display: flex;
		background-color: $uni-bg-color-title;
		color: #fff;
		.icon-left{
			padding-top: 15upx;
		}
		.title{
			width: 85%;
			text-align: center;
		}
	}
</style>
