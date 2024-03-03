<template>
	<view>
		<!-- <view class="empty" v-if="show==false">
			<view class="empty-text">空空如也</view>
			<view class="empty-button" @click="goshopping">去选购</view>
		</view> -->
		<view v-if="show==true">
			<view class="goods-detail" v-for="(item,index) in goods" :key="index">
				<view class="detail-left">
					<view class="goods-left">
						<checkbox-group @click="selected(item) " >
							<label>
								<view v-if="item.flag" ><image src="../../static/选择按钮 (已选择).png" class="xuanze"></image></view>
								<view v-else ><image src="../../static/选择按钮(未选).png" class="xuanze"></image></view>
							</label>
						</checkbox-group>
						  <image  :src="image_url+item.code" class="img"></image>
					</view>
					<view class="size">
					    <text class="name">{{item.name}}</text>
						<text class="msg">{{item.message}}</text>
					    <text class="goods-price">￥{{item.price}}/{{item.unit}}</text>
					</view>
				</view>
				<view class="right">
					<view class="detail-right">
						<text class="subtract" @click="reduce(item,index)">-</text>
						<text class="num">{{item.num}}</text>
						<text @click="add(item)" class="add">+</text>
					</view>
					
					<view class="delete"  @click="shanchu(index)" >
						<image src="../../static/删除.png"  class="shanchu" mode="scaleToFill"></image>
						
					</view>
				</view>
			</view>
		</view>
		<view class="end">
			<view class="end-left">
				<checkbox-group @click="selectgoods()">
					<label>
						<view v-if="allchecked" ><image src="../../static/选择按钮 (已选择).png" class="quanxuan"></image></view>
						<view v-else ><image src="../../static/选择按钮(未选).png" class="quanxuan"></image></view>
					</label>
				</checkbox-group>
				<view>
					合计：<text>￥ {{totalPrice}}</text>
				</view>
				</view>
				<view class="end-right">		    
					<button  @click="createOrder()">去结算({{totalNum}})</button>
				</view>
		</view>
	</view>
</template>

<script>
	
	import{
		recommend
	} from '../../network/http.js';
	
	export default{
		data(){
			return{
				show:true,
				allchecked:true,
				checked:true,
				goods:[],
				image_url:'',
				
			}
		},
	onLoad(){
		this.getPromoteGoods()		
		this.image_url='http://111.229.3.64:8184/goods/queryGoodsLogo?code='
	},	
		methods:{
			// goshopping(){
			// 	uni.navigateTo({ 
			// 		url:''
			// 	})
			// },
			
			async getPromoteGoods() {
				await recommend().then(res =>{
					console.log(this);
					const code = res.statusCode;
					if(code === 200){
						this.goods=res.data.data;
						this.goods.map(item=>{
							this.$set(item,'flag',true)
							this.$set(item,'num',1)
						})
					}
					else{
						uni.showToast({
							title:res.data.msg
						})
					}
				}).catch(err =>{
					console.log(err)
				})
			},
			
			
			selected(item){
				item.flag=!item.flag;	
				let a = true;
				this.goods.forEach((x)=>{
					if(x.flag===false) a=false;
				})
				if(a){
					this.allchecked=true
				}else{
					this.allchecked=false
				}
			},
			selectgoods(){
				this.allchecked=!this.allchecked
				if(this.allchecked){
					this.goods.map(item=>{
						this.checked=true						
						item.flag=true
					})
				}else{
					this.goods.map(item=>{
						this.checked=false
						item.flag=false
					})
				}

			},
			reduce(item,index){
				let num=item.num
				if(num>1){
					num-=1
				}
				item.num=num
			},
			shanchu(index){
				this.goods.splice(index,1)
			},
			add(item){
				let num =item.num
				item.num=num+1
			},
			// createOrder(){
			// 	uni.navigateTo({
			// 		url: '/pages/order/order'
			// 	});
			// },
			createOrder(){
				let list = this.goods;
				let goodsData = [];
				list.forEach(item=>{
					if(item.flag){
						goodsData.push({
							name:item.name,
							num:item.num,
							price:item.price,
							message:item.message,
							unit:item.unit,
							code:item.code,
						})
					}
				})
				
				uni.navigateTo({
					url: '../order/order?n='+JSON.stringify(goodsData)
				})
			}
		},
		computed:{
			totalNum(){
				let totalNum = 0;
				this.goods.map(item => {
				    item.flag ? totalNum += item.num : totalNum += 0
				})
				return totalNum
			},
			
			totalPrice() {
			    let totalPrice = 0;
			    this.goods.map(item => {
			        item.flag ? totalPrice += item.num * item.price : totalPrice += 0
			    })
			    return totalPrice
			},
			
			
		}
	}
	
</script>

<style lang="scss">
	page{
		background-color: #f5f5f5;
	}
	.goods{
		line-height: 80rpx;
		background-color: #fff;
		&-detail{
			
		    display: flex;
		    padding: 30rpx 15rpx 30rpx 30rpx;
		    background-color: #fff;
		    justify-content: space-between;
		    border-bottom: 5rpx solid #F1F1F1;
		    align-items: center;
		    .detail-left{
		        display: flex;
		        .goods-left{
		            display: flex;
		            align-items: center;
					.selected{
						color: #F1F1F1;
					}
					.img{
						width: 140rpx;
						height: 140rpx;
					}
					.xuanze{
						width: 40rpx;
						height: 40rpx;
						margin-right: 30rpx;
					}
		        }
		    }
		    .size{
		        display: flex;
		        justify-content: space-around;
		        flex-direction: column;
		        margin-left: 30rpx;
				
		        .goods-price{
		            font-size: 30rpx;
		            color: #F44545;
		            font-weight: bold;
		        }
				.name{
					font-size: 30rpx; 
					font-weight: bold;
					overflow: hidden;
					word-break: break-all;
					text-overflow: ellipsis; 
					display: -webkit-box;
					-webkit-box-orient: vertical;
					-webkit-line-clamp: 1;
				}
				.msg{
					font-size: 25rpx; 
					color:darkgrey;
					overflow: hidden;
					word-break: break-all;
					text-overflow: ellipsis; 
					display: -webkit-box;
					-webkit-box-orient: vertical;
					-webkit-line-clamp: 1;		
				}
		    } 
		}
		
	}
	
	
	.right{
		align-items: center;
		width: 150rpx;
		flex-direction: column;
	}
	.detail-right{
		padding-left: 40rpx;
		display: flex;
		width: 150rpx;
		align-items: center;
		 text{
		    line-height: 50rpx;
		    text-align: center;
		    display: inline-block;
		    background-color: #F7F7F7;
		    margin-right: 10rpx;
		}
		.num{
			font-size: 30rpx;
			font-weight: bold;
			border-radius: 20rpx;
		}
		.add {
		    color: #000000;		           
		     margin-right: 20rpx;
			background-color: #fff;
		}
		.subtract{
			color: #000000;
			background-color: #fff;
		}
		
		
		}
		.delete{
			margin-top: 20rpx;
			width: 50rpx;
			align-items: center;
			.shanchu{
				padding-left: 55rpx;
			width: 40rpx;
			height: 40rpx;
		}	
		
		
	}
	// .empty{
	// 	    position: relative;
	// 	    top: 220rpx;
	// 	    text-align: center;
	// 	    display: flex;
	// 	    align-items: center;
	// 	    flex-direction: column;
	// 	    &-text{
	// 	        color: #808080;
	// 	        margin-bottom: 50rpx;
	// 	    }
	// 	    &-button{
	// 	        width: 300rpx;
	// 	        height: 90rpx;
	// 	        color:orange;
	// 	        border: 1rpx solid orange;
	// 	        text-align: center;
	// 	        line-height: 90rpx;
	// 	        border-radius: 48rpx;
	// 	    }		
	// }
	.end{
	    height: 150rpx;
	    background-color:#fff;
	    position: fixed;
	    bottom: 0rpx;
	    left: 0;
	    display: flex;
	    align-items: center;
		width: 750rpx;
	    &-left{
	        display: flex;
	        justify-content: space-between;
	        padding: 0 30rpx;
			width: 440rpx;
	        .end-flex{
	            display: flex;
	            align-items: center;
	        }
			text{
				color: #f00;
				font-weight: bold;
			}
			.quanxuan{
				
				width: 40rpx;
				height: 40rpx;
				margin-right: 30rpx;
				
			}
	    }
	    &-right{
			button{
				
				margin-right: 15rpx;
				line-height: 90rpx;
				background-color: #516f25;
				text-align: center;
				font-size: 30rpx;
				font-weight: bold;
				color: #fff;
				border-radius: 50rpx;
				width: 250rpx;
			}
			
		}
	}
</style>
