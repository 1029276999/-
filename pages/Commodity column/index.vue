
<template>
	<view class="page-body" :style="'height:' + height + 'px'">


<!-- <drag-button <drag-button :isDock="true" :existTabBar="true" @btnClick="btnClick" @btnTouchstart="btnTouchstart"
			@btnTouchend="btnTouchend">
></drag-button> -->

<!-- <cover-view> -->
<view>
		<uni-fab class="aa"
			:pattern="pattern"
			:content="content"
			horizontal="right"
			vertical="bottom"
			:direction="direction"
			@fabClick="showDrawer"
			buttonColor="red"
		></uni-fab>
	</view>

<!-- <button class="aa" @click="showDrawer" type="primary">右侧弹出 显示Drawer</button> -->
		
		
		<uni-drawer ref="showRight" mode="left" :mask-click="false">




			<scroll-view style="height: 100%;" scroll-y="true">
				<button @click="closeDrawer" type="primary">返回</button>

				<scroll-view class="nav-left" scroll-y :style="'height:' + height + 'px'" :scroll-top="scrollLeftTop"
					scroll-with-animation>
					
					
					<view class="nav-left-item" @click="categoryClickMain(index),closeDrawer()" :key="index"
						:class="index == categoryActive ? 'active' : ''" v-for="(item, index) in classifyData">
						{{ item.name }}
					</view>
					
					
				</scroll-view>


			</scroll-view>


		</uni-drawer>

	<!-- 	</cover-view> -->

		<!-- <button @click="showDrawer" type="primary" class="a">右侧弹出 显示Drawer</button> -->







		<scroll-view class="nav-right" scroll-y :scroll-top="scrollTop" @scroll="scroll"
			:style="'height:' + height + 'px'" scroll-with-animation>
			<view v-for="(foods, index) in classifyData" :key="index" class="box">
				<view>{{foods.name}}</view>
				<view :id="i == 0 ? 'first' : ''" class="commodity" v-for="(item, i) in foods.foods" :key="i"
					@click="cart(item.name)">
					<view class="commodity-item">
						<image class='commodity-img' :src="item.Image" mode=""></image>
						<view class='commodity-content'>
							<view class='commodity-c1'>
								<text class='commodity-name'>{{item.name}}</text>

								<view>
									<text class='commodity-price'>¥{{item.price}}</text>
									<text class='commodity-soldnum'>已售：{{item.soldnum}}件</text>
								</view>
							</view>
							<view>
								<button class='commodity-bottom' hover-class="commodity-bgclick">加入购物车</button>
							</view>
						</view>
					</view>



				</view>
			</view>
		</scroll-view>






	</view>
</template>

<script>
	// import Base64 from '../../common/base64.js'
import { Base64 } from '../../js_sdk/js-base64';
	import classifyData from './cdata.js';
	import dragButton from '../../components/drag-button.vue';
	import {
		recommend,
		classification,
		classcontent
	} from '../../network/http.js';
	export default {
		components: {
					dragButton
				},

		data() {
			return {
							pattern: {
								color: 'gray',
								backgroundColor: '#FFFFFF',
								selectedColor: '#007AFF',
								buttonColor:'#81c561'
							},
			
				name: 'wkiwi',
				flbiao:[],
				t:[],
				g:[],
				i :0,
				height: 0,
				categoryActive: [],
				scrollTop: 0,
				scrollLeftTop: 0,
				// scrollHeight: 0,
				classifyData: classifyData,
				arr: [0, 584, 1168, 1752, 2336, 2805, 3274, 3858, 4442, 4911, 5380, 5734, 6203, 6672,
				7017], //初始值，后边计算会根据手机适配覆盖
				leftItemHeight: 51, //49行会计算出新值进行覆盖
				navLeftHeight: 0, //左边scroll-view 内层nav的总高度
				diff: 0, //左边scroll-view 内层nav的总高度与视口之差
				tabBarHeight: 0 //如果此页面为Tab页面，自己改变高度值,,一般tab高度为51
			};
		},
		created() {
			//如果你的分类数据为后台异步获取请	将下方代码放置你的数据回调中
			// this.$nextTick(()=>{
			// 	this.getHeightList();
			// })
		},
		onLoad: function() {
			this.height = uni.getSystemInfoSync().windowHeight - this.tabBarHeight;
		},
		onReady() {
			this.getHeightList();
			this.tj(1000000);
			this.makelist(0);
			this.findclasscontent('手机',1);
		},
		methods: {
			makelist(i){
			this.tj(0);
			// while(++i < this.flbiao.length){
			// console.log(this.classifyData[1])
			// while(++i < this.flbiao.length){

			// 		let keywords = this.classifyData[i].name;
					
			// 		this.findclasscontent(keywords,1);
					
			// 								}
				
			
			// let keywords = '手机';
			
			// this.findclasscontent(keywords);
			// }
			},
			async tj(i) {
				await classification().then(res => {
					const code = res.statusCode;
					if (code === 200) {
						this.flbiao = res.data.data;
						
						while(i++ < this.flbiao.length){
							this.t.push(this.flbiao[i]);
							this.classifyData.push(this.flbiao[i]);
							let keywords = this.classifyData[i].name;
							console.log('i'+i)
							console.log('keywords='+keywords)
							this.findclasscontent(keywords);
							let m=this.g
							console.log(m)
							this.classifyData[i].foods=this.g;
							
							// console.log(keywords)
													}
													
						
					} else {
						uni.showToast({
							title: res.data.msg
						})
					}
					
					
					
					
				}).catch(err => {
					console.log(err)
				})
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
			async findclasscontent(it) {
				let a=encodeURIComponent(it);
				await classcontent(a).then(res => {
					const code = res.statusCode;
					if (code === 200) {
						console.log(res.data.data)
					this.g = res.data.data;
					
					} else {
						uni.showToast({
							title: res.data.msg
						})
					}
				}).catch(err => {
					console.log(err)
				})
			},
			showDrawer() {
				this.$refs.showRight.open();
			},
			closeDrawer() {
				console.log(this.t);
				this.$refs.showRight.close();
			},
			getHeightList() {
				let _this = this;
				let selectorQuery = uni.createSelectorQuery();
				selectorQuery.selectAll('.nav-left-item').boundingClientRect(function(rects) {
					_this.leftItemHeight = rects[0].height;
					_this.navLeftHeight = _this.leftItemHeight * classifyData.length;
					_this.diff = _this.navLeftHeight - _this.height;
				});
				selectorQuery
					.selectAll('.box')
					.boundingClientRect(function(rects) {
						let arr = [0];
						let top = 0;
						rects.forEach(function(rect) {

							top += rect.height;
							arr.push(top);
						});
						// console.log(arr);
						_this.arr = arr;
					})
					.exec();
			},
			scroll(e) {
				let _this = this;
				if (this.timeoutId) {
					clearTimeout(this.timeoutId);
				}
				this.timeoutId = setTimeout(function() {
					//节流
					_this.scrollHeight = e.detail.scrollTop + 1 + _this.height / 2;
					//+1不要删除，解决最后一项某种情况下翻到底部，左边按钮并不会切换至最后一个
					//若想使切换参考线为屏幕顶部请删除 _this.height/2
					for (let i = 0; i < _this.arr.length; i++) {
						let height1 = _this.arr[i];
						let height2 = _this.arr[i + 1];
						if (!height2 || (_this.scrollHeight >= height1 && _this.scrollHeight < height2)) {
							_this.categoryActive = i;
							_this.diff > 0 && (_this.scrollLeftTop = Math.round((_this.categoryActive * _this
								.diff) / (classifyData.length - 1)));
							return false;
						}
					}
					_this.categoryActive = 0;
					_this.timeoutId = undefined;
				}, 10);
			},
			categoryClickMain(index) {
				this.categoryActive = index;
				this.scrollTop == this.arr[index] ? (this.scrollTop = this.scrollTop + 1) : (this.scrollTop = this.arr[
					index]); //防止两次相等造成点击不触发滚动时间
			},

		},
		
	};
</script>





<style lang="scss">
.commodity {
		&-item {
			border-radius: 25px;
			height: 530rpx;
			width: 730rpx;
			margin-bottom: 30rpx;
			border: 3rpx solid #aa9d6d;
			background-color: #fffce1;
		}
		&-img {
			border-top-left-radius: 25px;
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
.aa{

}

	.page-body {
		display: flex;
		background: #fff;
		overflow: hidden;
	}

	.nav {
		display: flex;
		width: 100%;
	}

	.nav-left {
		
		width: 500rpx;
		height: 100rpx;
		background: #fafafa;
		top: 100rpx;
		right: 0;
		// bottom: 0;
		// left: 0;
	}

	.nav-left-item {
		height: 100upx;
		border-right: solid 1px #f1f1f1;
		border-bottom: solid 1px #f1f1f1;
		font-size: 30upx;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.nav-left-item:last-child {
		border-bottom: none;
	}

	.nav-right {
		width: 750rpx;
		background-color: #96d1b0;
	}

	.box {
		display: block;
		overflow: hidden;
		border-bottom: 20upx solid #f3f3f3;
		/* min-height: 100vh; */
		/*若您的子分类过少想使得每个子分类占满屏请放开上边注视 */
	}

	.box:last-child {
		border: none;
		min-height: 100vh;
	}

	.nav-right-item {
		width: 28%;

		float: left;
		text-align: center;
		padding: 11upx;
		font-size: 28upx;
		background: #fff;
	}

	// .nav-right-item image {
	// 	width: 150upx;
	// 	height: 150upx;
	// }

	.active {
		color: #96d1b0;
		background: #f2ffed;
		border-right: 0;
	}

	::-webkit-scrollbar {
		/*取消小程序的默认导航条样式*/
		width: 0;
		height: 0;
		color: transparent;
	}
</style>
