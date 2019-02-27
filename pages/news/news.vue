<template>
	<view>
		<view class="list" v-for="(item,index) in newsList" :key="index" @click="goDetail(index)">
			<view class="list-left">
				<image :src="item.img"></image>
			</view>
			<view class="list-right">
				<view class="list-top">
					<text class="elli">{{item.title}}</text>
					<text>{{item.public_time}}</text>
				</view>
				<view class="list-con elli3">{{item.content}}</view>
			</view>
		</view>
		<text class="loading-text">
		{{loadingType === 0 ? contentText.contentdown : (loadingType === 1 ? contentText.contentrefresh : contentText.contentnomore)}}</text>
	</view>
</template>

<script>
	import uniLoadMore from '../../components/uni-load-more.vue';
	var  _self,
	page = 1;
	export default{
		components: {uniLoadMore},
		data(){
			return{
				newsList:[],
				loadingType: 0,
				contentText: {
					contentdown: "上拉显示更多",
					contentrefresh: "正在加载...",
					contentnomore: "没有更多数据了"
				}
			}
		},
		 onLoad: function (options) {
			_self = this;
			this.getNewsList(0);
		},
		// 下拉刷新
		onPullDownRefresh() {
			this.getNewsList(1);
		},
		// 上拉加载
		onReachBottom: function() {
			// console.log(_self.newsList.length);
			if (_self.loadingType != 0) {//loadingType!=0;直接返回
				return false;
			}
			_self.loadingType = 1;
			uni.showNavigationBarLoading();//显示加载动画
			uni.request({
				url:'../../static/data/news.json?page='+page,
				success: function(res) {
					if (_self.newsList.length==res.data.pages_count) {//没有数据
						_self.loadingType = 2;
						uni.hideNavigationBarLoading();//关闭加载动画
						return false;
					}
					page++;//每触底一次 page +1
					// console.log(page);
					for(var i=_self.newsList.length;i<res.data.pages_count;i++){
						_self.newsList = _self.newsList.concat(res.data.data[i]);//将数据拼接在一起
					}
					_self.loadingType = 0;//将loadingType归0重置
					uni.hideNavigationBarLoading();//关闭加载动画
					
				}
			});
		},
		methods:{	
			getNewsList: function(options) {//第一次回去数据
				_self.loadingType = 0;
				uni.showNavigationBarLoading();
				uni.request({
					url: '../../static/data/news.json?page=1',
					success: function(res) {
						let newsList =[];
						for(var i=0;i<6;i++){
							newsList.push(res.data.data[i]);
						}
						_self.newsList = newsList;
						if(res.data.pages_count==res.data.data.length&&options==1){
							uni.showToast({
								title: '已是最新',
								duration: 2000
							});
						}
						uni.hideNavigationBarLoading();
						uni.stopPullDownRefresh();//得到数据后停止下拉刷新
					}
				});
			},
			goDetail: function(e){
				console.log(e);
				uni.navigateTo({
					url: '../details/details?id='+e
				})
			}
		}
	}
</script>

<style lang="scss">
	@import '../../common/public.css';
	.list{
		width: 94%;
		display: flex;
		flex-direction: row;
		padding: 3%;
		border-bottom: 1upx solid #ebe8e8;
		.list-left{
			width: 35%;
			margin-right: 3%;
			image{
				width: 220upx;
				height: 140upx;
				padding: 3%;
				border: 1upx solid #ebe8e8;
				box-sizing: border-box;
			}
		}
		.list-right{
			width: 65%;
			color: #999999;
			font-size: 24upx;
			.list-top{
				display: flex;
				flex-direction: row;
				justify-content: space-between;
				padding-bottom: 5upx;
				text{
					&:nth-child(1){
						color: #333;
						font-size: 30upx;
						width: 65%;
					}
				}
			}
		}
	}
	.loading-text{
		height: 80upx;
		line-height: 80upx;
		font-size: 30upx;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
	}
</style>
