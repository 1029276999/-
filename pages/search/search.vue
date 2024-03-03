<template>
	</view> 
	<view class="my">
		
		<!-- 头部 -->
		<view>
			<view class="top">
				<view class="center">
					<view class="center_top">
						<view class="center_img" @tap="gotoFeeds('')">
							<!-- 这里可以放自己的静态头像 -->
							<image class="logo-img" src="/static/logo.png" mode=""></image>
						</view>
						<view class="center_info" @tap="gotoFeeds('')">
							<view class="center_vip">
								<image class="rank_icon" src="" />
								<view class="vip_text">
									<text>146****8006</text>
								</view>
							</view>
						</view>
						<!-- 设置 -->
						<view style="margin-left: 140rpx;margin-top: 15rpx;"  @tap="gotoFeeds('')">
							<image style="width: 30px;height: 30px;" src=""></image>
						</view>
					</view>
					<view class="center_down">
						<view class="center_score" @tap="toNone">
							<text class="rank_text">我的积分</text>
						</view>
						<view class="logo-name">3911</view>
					</view>
				</view>
				<image src='/static/liudong.gif' mode='scaleToFill' class='gif-wave'></image>
			</view>
		</view>
			
		<!-- 我的订单 -->
		<view class="order">
			<view class="order-title">
				<view class="">我的订单</view>
				<view class="">查看全部订单 > </view>
			</view>
			<view class="order-list">
				<view class="order-item">
					<image class="order-img" src="/static/logo.png" mode=""></image>
					<view class="">待付款</view>
				</view>
				<view class="order-item">
					<image class="order-img" src="/static/logo.png" mode=""></image>
					<view class="">待发货</view>
				</view>
				<view class="order-item">
					<image class="order-img" src="/static/logo.png" mode=""></image>
					<view class="">待收货</view>
				</view>
				<view class="order-item">
					<image class="order-img" src="/static/logo.png" mode=""></image>
					<view class="">评价</view>
				</view>
				<view class="order-item">
					<image class="order-img" src="/static/logo.png" mode=""></image>
					<view class="">售后</view>
				</view>
			</view>
			<!-- 内容列表 -->
			<view class="my-content">
				<view class="my-content-item" v-for="(item,index) in fction">
					<view class="">
						{{item.fct}}
					</view>
					<view class="">
						>
					</view>
				</view>
				<!-- <view class="my-content-item" v-for="(item,index) in 1">
					<view class="">
						
					</view>
					<view class="">
						>
					</view>
				</view>
				<view class="my-content-item" v-for="(item,index) in 1">
					<view class="">
						
					</view>
					<view class="">
						>
					</view>
				</view> -->
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				fction:[
					{
						fct:'收货地址',
					},
					{
						fct:'客服聊天',
					},
					{
						fct:'账号设置',
					},
				]
			}
		},
		methods: {
			
		}
	}
</script>

<style lang="scss">
	.top {
		width: 100%;
		height: 130px;
		background: #185222;
		padding-top: 15px;
		position: relative;
	}
	
	.center {
		width: 95%;
		height: 100px;
		background: white;
		display: flex;
		flex-direction: column;
		margin: 0 auto;
		border-radius: 5px;
	}
	
	.center_top {
		display: flex;
		flex-direction: row;
		width: 80%;
		height: 80px;
		margin: 0 auto;
		margin-top: 20rpx;
		border-bottom: 1px solid #EEEEEE;
	}
	
	.center_img {
		width: 120rpx;
		height: 120rpx;
		border:2rpx solid #ccc;
		border-radius: 50%;
		background-color: #FFFFFF;
	}
	
	.center_img image {
		width: 120rpx;
		height: 120rpx;
		// border-radius: 50%;
	}
	
	.center_img .user_head {
		width: 120rpx;
		height: 120rpx;
	}
	
	.center_info {
		display: flex;
		flex-direction: column;
		margin-top: 20rpx;
		margin-left: 30px;
	}
	
	.center_name {
		font-size: 20px;
	}
	
	.center_phone {
		color: #BEBEBE;
	}
	
	.center_down {
		display: flex;
		flex-direction: row;
		width: 80%;
		height: 35px;
		margin: 0 auto;
		margin-top: 20rpx;
	}
	
	.center_rank {
		width: 50%;
		height: 35px;
		display: flex;
		flex-direction: row;
	}
	
	.rank_text {
		height: 35px;
		line-height: 35px;
		margin-left: 10rpx;
		color: #AAAAAA;
	}
	
	.center_vip image {
		width: 25px;
		height: 25px;
		margin-top: 15rpx;
	}
	
	.vip_icon {
		width: 25px;
		height: 25px;
		margin-top: -10rpx;
	}
	
	.vip_text {
		margin-top: -55rpx;
		margin-left: 50rpx;
		color: #AAAAAA;
	}
	
	.center_rank image {
		width: 35px;
		height: 35px;
	}
	
	.center_score {
		width: 50%;
		height: 35px;
		display: flex;
		flex-direction: row;
	}
	
	.center_score image {
		width: 35px;
		height: 35px;
	}
	
	.gif-wave {
		position: absolute;
		width: 100%;
		bottom: 0;
		left: 0;
		z-index: 99;
		mix-blend-mode: screen;
		height: 100rpx;
	}
	.my-header{
	    // background: url("/static/logo.png") no-repeat;
	    width: 100%;
	    height: 400rpx;
	}
	.header-main{
	    position:relative;
	    top: 120rpx;
	}
	.header-config{
	    position: absolute;
	    left: 20rpx;
	}
	.header-logo{
	    position: absolute;
	    left:50%;
	    margin-left:-60rpx;
	    width: 120rpx;
	}
	.config-img{
	    width: 120rpx;
	    height: 120rpx;
	    border:2rpx solid #ccc;
	    border-radius: 50%;
	    background-color: #FFFFFF;
	}
	.logo-img{
	    width: 120rpx;
	    height: 120rpx;
	    border:2rpx solid #ccc;
	    border-radius: 50%;
	    background-color: #FFFFFF;
	}
	.logo-name{
		width: 400rpx;
	    color: #00aa00;
	    font-size: 60rpx;
	    font-weight: bold;
	}
	.order-title{
	    display: flex;
	    justify-content: space-between;
	    align-items: center;
	    padding: 20rpx;
	}
	.order-list{
	    padding:20rpx;
	    display: flex;
	}
	.order-item{
	    flex:1;
	    display: flex;
	    flex-direction: column;
	    justify-content: center;
	    align-items: center;
	}
	.order-img{
	    width: 80rpx;
	    height: 80rpx;
	}
	.my-content{
	    margin:20rpx 0;
	    padding: 0 20rpx;
	}
	.my-content-item{
	    display: flex;
	    justify-content: space-between;
	    align-items: center;
	    padding: 20rpx 0;
	    border-bottom: 2px solid #ccc;
	}
	.catd {
		overflow: hidden;
	}
</style>