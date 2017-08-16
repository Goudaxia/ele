<template lang="html">
	<div>
		<div class="cart" @click="toggleList">
		<div class="cart-left">
			<div class="cart-ourter">
				<div class="cart-inner" :class="{active:count}">
					<i class="iconfont icon-gouwuche"></i>
				</div>
				<div class="count" v-show="count">{{count}}</div>
			</div>
		</div>
		<div class="cart-md">
			<strong>￥{{total}}</strong>
			<span>另需配送费￥{{deliveryPrice}}</span>
		</div>
		<div class="cart-right" :class="{active:total>=minPrice}">
			{{payDesc}}
		</div>
		</div>
		
		<transition name="slide">
			<div class="cart-list" v-show="show && count">
				<div class="cart-list-hd">
					<h2>购物车</h2>
					<span @click="clear">清空</span>
				</div>
				<div class="cart-list-bd">
					<ul>
						<li v-for="(food,index) in cartGoods" :key="index">
							<h3>{{food.name}}</h3>
							<strong>￥{{food.price}}</strong>
							<div class="ele-buy">
								<ele-buy :food="food"></ele-buy>
							</div>
						</li>
					</ul>
				</div>
			</div>
		</transition>
		
		<transition name="fade">
			<div class="mask" v-show="show && count"></div>
		</transition>
		
	</div>
</template>

<script>
import Buy from '../Buy/Buy'
export default {
	data(){
		return {
			show:false
		}
	},
	methods:{
		toggleList(){
			if(this.count){
				this.show=!this.show;
			}
		},
		clear(){
			this.cartGoods.forEach(item=>item.count=0);
			console.log(this.show)
			this.show=false;
			console.log(this.show)
		}
	},
	props:{
		cartGoods:{
			type:Array,
			required:true
		},
		minPrice:{
			type:Number,
			required:true
		},
		deliveryPrice:{
			type:Number,
			required:true
		}
	},
	computed:{
		count(){
			// let cut=0;
			// this.cartGoods.forEach(item=>{
			// 	cut+=item.count;
			// });
			// return cut;
			let cnt = 0;
            this.cartGoods.forEach(item => {
                cnt += +item.count;
            });
            return cnt;
		},
		total(){
			let sum=0;
			this.cartGoods.forEach(item=>{
				sum+=item.count*item.price;
			});
			return sum;
		},
		payDesc(){
			if(!this.count){
				return `￥${this.minPrice}元起送`;
			}else if(this.total<this.minPrice){
				let dif=this.minPrice-this.total;
				return `还差￥${dif}元起送`;
			}else{
				return `去结算`;
			}
		}
	},
	components:{
		'ele-buy':Buy
	}
}
</script>

<style lang="less" scoped>

.slide-enter,.slide-leave-active{transform:translateY(100%);}
.slide-enter-active,.slide-leave-active{transition:all 0.5s;}
.fade-enter,.fade-leave-active{opacity:0;}
.fade-enter-active,.fade-leave-active{transition:all 0.5s;}
	.cart{
		z-index:100;
		height:48px;
		position:fixed;
		left:0;
		right:0;
		bottom:0;
		background:#141d27;
		display:flex;
		align-items:center;
		.cart-left{
			width:80px;
			height:48px;
			position:relative;
			.cart-ourter{
				width:56px;
				height:56px;
				border-radius:50%;
				background:#141d27;
				position:absolute;
				left:12px;
				top:-10px;
				.cart-inner{
					width:44px;
					height:44px;
					border-radius:50%;
					background:rgba(255,255,255,0.2);
					position:absolute;
					top:6px;
					left:6px;
					text-align:center;
					line-height:44px;
					i{
						font-size:24px;
						color:rgba(255,255,255,.4);
					}
					&.active{
						background:rgb(0,160,220);
						i{
							color:rgb(255,255,255);
						}
					}
				}
				.count{
					position:absolute;
					right:0;
					top:0;
					width:20px;
					height:20px;
					border-radius:50%;
					background:rgb(240,20,20);
					font-size:10px;
					color:#fff;
					text-align:center;
					line-height:20px;
				}
			}
		}
		.cart-right{
			width:105px;
			height:48px;
			text-align:center;
			line-height:48px;
			color:rgba(255,255,255,0.4);
			font-size:12px;
			font-weight:700;
			&.active{
				background:#00b43c;
				color:#fff;
			}
		}
		.cart-md{
			flex-grow:1;
			font-size:16px;
			color:rgba(255,255,255,.4);
			strong{
				font-weight:700;
				border-right:1px solid rgba(255,255,255,.1);
				padding-right:12px;
				margin-right:12px;
			}
			span{

			}
		}
	}
	.cart-list{
		z-index:90;
		position:fixed;
		left:0;
		right:0;
		bottom:48px;
		background:#fff;
		.cart-list-hd{
			height:40px;
			padding:0 18px;
			background:#f3f5f7;
			line-height:40px;
			border-bottom:1px solid rgba(7,17,27,.1);
			position:relative;
			h2{
				font-size:14px;
				font-weight:200;
				color:rgb(7,17,27);
			}
			span{
				font-size:12px;
				color:rgb(0,160,220);
				position:absolute;
				right:18px;
				top:0;
			}
		}
		.cart-list-bd{
			padding:0 18px;
			li{
				padding:12px 0;
				border-bottom:1px solid rgba(7,17,27,.1);
				position:relative;

				h3{
					font-size:14px;
					color:rgb(7,17,27);
					line-height:24px;
				}
				strong{
					position:absolute;
					right:85px;
					top:16px;
					color:rgb(240,20,20);
					font-size:14px;
					font-weight:700;
				}
				.ele-buy{
					position:absolute;
					right:0;
					top:12px;
				}
			}
		}
	}
	.mask{
		z-index:80;
		position:fixed;
		top:0;
		left:0;
		right:0;
		bottom:48px;
		background:rgba(7,17,27,.6);
	}
</style>