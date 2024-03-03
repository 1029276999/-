<template>
	<view class="content">
		<view class="kong"> </view>
		<view class="status_bar">
			<view class="map">
				<view class="right-view">
					<view class="text">
						武汉
					</view>
				</view>
				<view class="left-view" @click="fideit">
					<input v-model="value" type="number" class="input" placeholder="搜索特产:">
				</view>
			</view>
		</view>





		<view class="lbt">
			<swiper class="lbt-i" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
				:duration="duration">
				<swiper-item v-for="(item, index) in info" :key="index">
					<view :class="lbt-i" class="swiper-item">
						<image class="lbt-i" :src="item.url" mode="aspectFill" />
					</view>
				</swiper-item>
			</swiper>
		</view>
		<view class="cate-section">
			<view class="cate-item">
				<image class="cate-img" src="/static/temp/c8.png"></image>
				<text>茶叶茶饼</text>
			</view>
			<view class="cate-item">
				<image class="cate-img" src="/static/temp/c7.png"></image>
				<text>农家风味</text>
			</view>
			<view class="cate-item">
				<image class="cate-img" src="/static/temp/c3.png"></image>
				<text>水果特产</text>
			</view>
			<view class="cate-item">
				<image class="cate-img" src="/static/temp/c1.png"></image>
				<text>海鲜风味</text>
			</view>
		</view>
		<view class="guodu">
			<image class="x" src="/static/左.png">
			</image>
			<view class="te">今日特价</view>
			<image class="x" src="/static/右.png">
			</image>
		</view>
		<!-- 过渡 -->


		<view class='commodity'>
			<!-- 单个商品组件 -->

			<view class='commodity-item' v-for="(item,index) in loadm.data" :key='index'>
				<image class='commodity-img' :src="image_url.de+item.code" mode="" ></image>
				<view class='commodity-content'>
					<view class='commodity-c1'>
						<text class='commodity-name'>{{item.name}}</text>

						<view>
							<text class='commodity-price'>¥{{item.price}}</text>
							<view>
								<text class='commodity-soldnum'>已售：{{item.soldnum}}件</text>
							</view>
						</view>
					</view>
					<view @click="cc(item.code)">
						<view class='commodity-an'>
						<button class='commodity-bottom' hover-class="commodity-bgclick">加入购物车</button>
						</view>
					</view>

				</view>


			</view>

		</view>


	</view>


</template>

<script>
	import {
		recommend,
	} from '../../network/http.js';
	export default {


		data() {
			return {
				title: '该文字为商品描述',
				indicatorDots: true,
				autoplay: true,
				interval: 4000, //切换的间隔时间
				duration: 500, //滑动动画时长	
				image_url:{
					type:String,
					de:'http://111.229.3.64:8184/goods/queryGoodsLogo?code=',
					},
				info: [{
						url: '/static/tea.jpeg',
					},
					{
						url: '/static/tea1.jpeg',
					},
					{
						url: '/static/tea2.jpeg',
					}
				],
				loadm: [],

			}
		},
		onLoad() {
		
			this.sp();
			
		},


		methods: {
			
			fideit(){		
				uni.navigateTo({
					url: '../HM-search/HM-search'
				})
			},
			 cc(code){
			 	console.log(code);
			 },
			async sp() {
				await recommend().then(res => {
					
					const code = res.statusCode;
					if (code === 200) {
						this.loadm = res.data;
					} else {
						uni.showToast({
							title: res.data.msg
						})
					}
				}).catch(err => {
					console.log(err)
				})
			},
			
			


		},


	}
</script>

<style lang="scss">
	.kong{
		height: 50rpx;
	}
	.commodity {
		&-item {
			border-radius: 15px;
			height: 530rpx;
			width: 730rpx;
			margin-bottom: 30rpx;
			border: 3rpx solid #aa9d6d;
			background-color: #fffce1;
		}
		&-img {
			border-top-left-radius: 15px;
			border-top-right-radius: 25px;
			border: 3rpx solid #aa9d6d;
			height: 300rpx;
			width: 650rpx;
			margin-top: 20rpx;
			margin-left: 35rpx;
			margin-right: 30rpx;
		}
		&-content {
			display: flex;
			height: 200rpx;
			width: 650rpx;
			margin-left: 50rpx;
		}
		&-c1 {
			height: 200rpx;
			width: 350rpx;
		}
		&-name {
			font-weight: 600;
			width: 350rpx;
			font-size: 30rpx;
			margin-left: 30rpx;
		}
		&-price {
			font-weight: 600;
			margin-left: 10rpx;
			font-size: 60rpx;
			color: red;
		}
		&-bottom {
			border-right: 4rpx solid #2d2d2d;
			border-bottom: 5rpx solid #2f2f2f;
			margin-top: 100rpx;
			margin-left: 80rpx;
			font-size: 28rpx;
			color: white;
			height: 66rpx;
			width: 206rpx;
			background-color: #3C5E0B;
			opacity: .7;
		}
		&-bgclick {
			background-color: #203205;
		}
		&-soldnum {
			margin-top: 2rpx;
			margin-left: 30rpx;
			font-size: 28rpx;
			opacity: .7;
		}
	}
	.te {
		display: flex;
		flex-direction: row;
		align-items: center; //垂直居中
		margin-left: 60rpx;
		width: 300rpx;
	}
	.cate {
		&-section {
			width: 712rpx;
			position: relative;
			z-index: 5;
			border-radius: 16upx 16upx 0 0;
			margin-top: -20upx;
			display: flex;
			justify-content: space-around;
			align-items: center;
			flex-wrap: wrap;
			padding: 40upx 22upx;
			background: #f7fff0;
			font-size: 26rpx;
			border-top: 2rpx solid #89e3687D;
			border-bottom: 2rpx solid #8aaa57;
		}

		&-img {
			width: 88upx;
			height: 88upx;
			margin-bottom: 14upx;
			border-radius: 50%;
			opacity: .7;

		}

		&-item {
			display: flex;
			flex-direction: column;
			align-items: center;
			font-size: 28upx;
			color: black;
		}

	}

	.x {
		height: 88upx;
		width: 88upx;
		margin: auto;
	}

	.text {
		margin: auto;

	}

	.right-view {
		width: 70px;
		border-right: 1px solid;
		display: flex;
		background-color: #ebf8e0;
		margin-left: 0rpx;
		margin-right: 0rpx;
		padding-left: 0rpx;
	}

	.map {
		display: flex;
		height: 120rpx;
		margin-left: 0rpx;
		background-color: #ebf8e0;
		border-bottom: 1px solid;
	}

	.left-view {
		margin: 30rpx;
		margin-left: 30rpx;
		border-radius: 25px;
		display: flex;
		background-color: #f8f5f8;

	}

	.status_bar {
		height: var(--status-bar-height);
		width: 750rpx;
		background-color: #F8F8F8;
	}

	.content {
		background-color: #96d1b0;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		font-size: 38rpx;
	}

	.logo {
		height: 300rpx;
		width: 650rpx;
		margin-top: 20rpx;
		margin-left: 50rpx;
		margin-right: 50rpx;
		margin-bottom: 30rpx;
	}

	.guodu {


		display: flex;
		width: 450rpx;
		height: 100rpx;
		margin-left: 225rpx;
		margin-right: 225rpx;

	}

	.lbt {

		height: 400rpx;
		width: 750rpx;
		margin-top: 65rpx;
		margin-right: auto;
		
		border: 3px;


&-i{
	height: 400rpx;
	width: 750rpx;
}
	}
	.spx {
		border-radius: 25px;
		height: 500rpx;
		width: 750rpx;
		margin-bottom: 30rpx;
		border: 3px;
		background-color: beige;
	}

	.logo2 {
		height: 300rpx;
		width: 300rpx;
		margin-top: 50rpx;
		margin-left: 0rpx;

		margin-bottom: 30rpx;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
