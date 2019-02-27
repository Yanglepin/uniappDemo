
<template>
	<view>
		<!-- 头部 -->
		<view class="header">
			<image src="../../static/logo.png"></image>
		</view>
		<!-- 轮播图 -->
		<view class="swiper-view">
			<swiper class="swiper" indicator-dots="true" autoplay="true" circular="true" indicator-active-color="#10623e">
				<swiper-item v-for="swiper in swiperList" :key="swiper.sid">
					<image mode="aspectFill" :src="swiper.img"></image>
				</swiper-item>
			</swiper>
		</view>
		<!-- 分类 -->
		<view class="column-3">
			<block v-for="item in column" :key="item.id">
				<image :src="item.img" @click="toSort(item)"></image>
			</block>
		</view>
		<!-- 最新动态 -->
		<view class="news">
			<view class="left">
				<image src="../../static/dt.png"></image>
			</view>
			<view class="right">
				<view>
					<text>动态</text>
					<text class="elli">你好你好你好你好你好你好你好你好你好你好你好你好</text>
				</view>
				<view>
					<text>动态</text>
					<text class="elli">你好你好你好你好你好你好你好你好你好你好你好你好</text>
				</view>
			</view>
		</view>
		<!-- 名家作品 -->
		<view class="img">
			<image src="../../static/shuhua.png"></image>
		</view>
		<!-- 精品推荐 -->
		<view class="recommend">
			<view>精品推荐</view>
			<view>MORE</view>
		</view>
		<!-- 横向滑动  scroll-left设置横向滚动条的位置-->
		<view class="scroll-view_H">
			<scroll-view scroll-x="true" scroll-left="0">
				<view class="list" v-for="scroll in scrollList" :key="scroll.id">
					<image mode="aspectFill" :src="scroll.img"></image>
					<view class="elli2">{{scroll.title}}</view>
					<view class="elli">￥{{scroll.price}}</view>
					<view class="elli">{{scroll.size}}cm</view>
				</view>
			</scroll-view>
		</view>
		<!-- 精品翡翠 -->
		<view class="img">
			<image src="../../static/jpfc.png"></image>
		</view>
		<view class="product-list">
			<view class="product" v-for="product in productList" :key="product.id">
				<view class="image-view">
					<image class="product-image" :src="product.img"></image>
				</view>
				<view class="product-title elli2">{{product.title}}</view>
				<view class="product-price">
					<text class="product-price-favour">￥{{product.originalPrice}}</text>
					<text class="product-price-original">￥{{product.price}}</text>
				</view>
				<view class="product-size">{{product.size}}</view>
			</view>
		</view>

	</view>
</template>
<style lang="scss">
	@import 'index.scss';
</style>
<script>
	export default {
		data() {
			return {
				//轮播
				swiperList:[
					{sid:0,src:'自定义src0',img:'../../static/banner.png'},
					{sid:1,src:'自定义src1',img:'../../static/banner1.png'},
					{sid:2,src:'自定义src2',img:'../../static/banner.png'},
					{sid:3,src:'自定义src3',img:'../../static/banner1.png'}
				],
				//横向滚动
				scrollList:[
					{id:0,title:'国家一级美术师 向士平 山水 国画',price:'1500.00',size:'136*68',img:'../../static/paint1.png'},
					{id:1,title:'国家一级美术师 向士平 山水 国画',price:'12345.00',size:'136*68',img:'../../static/paint2.png'},
					{id:2,title:'国家一级美术师 向士平 山水 国画',price:'2200.00',size:'136*68',img:'../../static/paint1.png'},
					{id:3,title:'国家一级美术师 向士平 山水 国画',price:'1300.00',size:'136*68',img:'../../static/paint2.png'},
				],
				//列表
				productList:[],
				column:[
					{id:1,img:'../../static/thjl.png'},
					{id:2,img:'../../static/zbzc.png'},
					{id:3,img:'../../static/zjyp.png'}
				]
			}
		},
		onLoad() {
			this.getProductList();
		},
		methods: {
			getProductList(){
				uni.request({
					url: '../../static/data/index.json', 
					success:(res) => {
						this.productList = res.data.data;
						// console.log(this.productList);
					},
					fail:(error) => {
						console.log(error);
					}
				})
			},
			toSort(e){
				// console.log(e.id);
			    uni.switchTab({
					url: '../sort/sort?id=1'
				});
			}
		}
	}
</script>


	
	

