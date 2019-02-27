<template>
	<view>
		<view class="scroll-tab">
			<scroll-view id="tab-bar" class="scroll-title"  scroll-x :scroll-left="scrollLeft">
				<view v-for="(tab,index) in tabBars" :key="tab.id" :class="['list',tabIndex==index ? 'active' : '']" :id="tab.id"
			 :data-current="index" @click="tapTab(index)">
					<text>{{tab.title}}</text>
				</view>
			</scroll-view>
			<swiper :current="tabIndex" class="swiper-box" duration="300" @change="changeTab">
				<swiper-item  v-for="(tab,index1) in newsitems" :key="index1">
					<scroll-view class="list" scroll-y @scrolltolower="loadMore(index1)">
						<view>{{tab.title}}</view>
					</scroll-view>
				</swiper-item>
			</swiper>
		</view>
	</view>
</template>
<script>
	export default{
		data(){
			return{
				scrollLeft: 0,
				isClickChange: false,
				tabIndex: 0,
				tabBars:[
					{"id":1,"title":"特惠捡漏"},
					{"id":2,"title":"珠宝专场"},
					{"id":3,"title":"宗教用品"}
				],
				newsitems:[
					{"id":1,"title":"特惠捡漏"},
					{"id":2,"title":"珠宝专场"},
					{"id":3,"title":"宗教用品"}
				]
			}
		},
		 onLoad: function (option) {
			console.log(option.id);
		},
		methods:{
			loadMore(e) {
				this.newsitems[e].loadingType = 1;
				setTimeout(() => {
					this.addData(e);
				}, 1200);
			},
			async changeTab(e) {
				let index = e.detail.current;
				if (this.isClickChange) {
					this.tabIndex = index;
					this.isClickChange = false;
					return;
				}
				let tabBar = await this.getElSize("tab-bar"),
					tabBarScrollLeft = tabBar.scrollLeft;
				let width = 0;
			
				for (let i = 0; i < index; i++) {
					let result = await this.getElSize(this.tabBars[i].id);
					width += result.width;
				}
				let winWidth = uni.getSystemInfoSync().windowWidth,
					nowElement = await this.getElSize(this.tabBars[index].id),
					nowWidth = nowElement.width;
				if (width + nowWidth - tabBarScrollLeft > winWidth) {
					this.scrollLeft = width + nowWidth - winWidth;
				}
				if (width < tabBarScrollLeft) {
					this.scrollLeft = width;
				}
				this.isClickChange = false;
				this.tabIndex = index; //一旦访问data就会出问题
			},
			getElSize(id) { //得到元素的size
				return new Promise((res, rej) => {
					uni.createSelectorQuery().select('#' + id).fields({
						size: true,
						scrollOffset: true
					}, (data) => {
						res(data);
					}).exec();
				});
			},
			async tapTab(index) { //点击tab-bar
				if (this.tabIndex === index) {
					return false;
				} else {
					let tabBar = await this.getElSize("tab-bar"),
						tabBarScrollLeft = tabBar.scrollLeft; //点击的时候记录并设置scrollLeft
					this.scrollLeft = tabBarScrollLeft;
					this.isClickChange = true;
					this.tabIndex = index;
				}
			}
		}
		
	}
</script>

<style lang="scss">
	.scroll-tab{
		width: 100%;
		font-size: 30upx;
		.scroll-title{
			width: 100%;
			// padding: 10upx 0upx;
			border-bottom: 1upx solid #e1e1e1;
			box-sizing: border-box;
			white-space: nowrap;
			.list{
				width: 33.3%;
				display: inline-block;
				text-align: center;
				height: 90upx;
				line-height: 90upx;
				box-sizing: border-box;
			}
			.active{
				text{
					color: #0e91ec;
					border-bottom: 1px solid #0e91ec;
				}
			}
		}
	}
</style>