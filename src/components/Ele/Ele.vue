<template lang="html">
	<div class="ele">
		<div class="goods">
			<div class="goods-menu"  id='goods-menu'>
				<ul>
					<li v-for="(item,index) in goods"  :key="index"   >
						<p>{{item.name}}</p>
					</li>
				</ul>
			</div>
			<div class="goods-list" id="goods-list">
				<div ref="list">
					<dl  v-for="(item,index) in goods" :key="index" ref="index">
						<dt>{{item.name}}</dt>
						<dd v-for="(food,idx) in item.foods" :key="idx" >
							<img :src="food.icon" alt="">
							<div class="food">
								<h2>{{food.name}}</h2>
								<p class="desc">{{food.description}}</p>
								<p class="sale">月售{{food.sellCount}}份 好评率{{food.rating}}</p>
								<p class="price">
									<strong><span>￥</span>{{food.price}}</strong>
									<del v-if="food.oldPrice">￥{{food.oldPrice}}</del>
								</p>
								<!-- <div class="ele-buy">
									<ele-buy :food="food"></ele-buy>
								</div> -->
							</div>
						</dd>
					</dl>
				</div>
			</div>
		<!-- 	<transition name="slide">
				<ele-food v-show="show" @close="show=false" :food="currentFood"></ele-food>
			</transition>
			<div class="ele-cart">
				<ele-cart :cartGoods="cartGoods" :minPrice="seller.minPrice" :deliveryPrice="seller.deliveryPrice"></ele-cart>
			</div> -->
		</div>
	</div>
</template>

<script>

import axios from 'axios'
import IScroll from '../../../static/js/iscroll-probe'

export default {
	data(){
		return {
			goods:[]
		}
	},
	created(){
		// axios.get('https://mainsite-restapi.ele.me/shopping/v2/menu?restaurant_id=1020107')
  //            .then(response => {
  //                this.goods = response.data;
  //                console.log(this.goods);
  //                //此处调用refresh，刷新一下，重新计算高度
  //               setTimeout(() => {
  //                   this.menuScroll.refresh();
  //                   this.listScroll.refresh();
  //               },10);
  //            })
  //            .catch(error => console.log(error));

		let url="https://mainsite-restapi.ele.me/shopping/v2/menu?restaurant_id=1020107";
		axios.get(url)
			.then(response=>{
				this.goods=response.data;
				console.log(this.goods);

				setTimeout(()=>{
					this.menuScroll.refresh();
					this.listScroll.refresh();
				},0);
				
			})
			.catch(error=>console.log(error));
			
	},
	mounted(){
		this.menuScroll=new IScroll("#goods-menu",{click:true});
		this.listScroll=new IScroll("#goods-list",{click:true,probeType:2})
	}
}	

</script>

<style lang="less">
.goods{
	
	position:fixed;
	display:flex;
	top:175px;
	left:0;
	right:0;
	bottom:48px;
	overflow:hidden;
	.goods-menu{
		width:80px;
		li{
			height:54px;
			background-color:#f3f5f7;
			padding-left:12px;
			padding-right:12px;
			&.active{
				background-color:#fff;
				p{
					border-bottom:0 none;
				}
			}
			p{
				height:53px;
				width:56px;
				border-bottom:1px solid rgba(7,17,27,0.1);
				display:flex;
				align-items:center;
				font-size:12px;
				font-weight:200;
				line-height:14px;
			}
		}
	}
	.goods-list{
		flex-grow:1;
		dt{
			height:26px;
			background-color:#f3f5f7;
			border-left:1px solid #d9dde1;
			font-size:12px;
			line-height:26px;
			color:rgb(147,153,159);
			padding-left:14px;
		}
		dd{
			padding:18px;
			display:flex;
			img{
				width:57px;
				height:57px;
				padding-right:10px;
			}
			.food{
				position:relative;
				display:flex;
				flex-grow:1;
				flex-direction:column;
				justify-content:space-between;
				h2{
					font-size:14px;
					color:rgb(7,17,27);
					line-heihgt:14px;
				}
				.desc,.sale{
					font-size:10px;
					color:rgb(147,153,159);
					line-height:10px;
					margin:8px 0;
				}
				.price{
					strong{
						font-size:14px;
						font-weight:700;
						line-height:24px;
						color:rgb(240,20,20);
						span{
							font-size:10px;
						}
					}
					del{
						font-size:10px;
						font-weight:700;
						line-height:24px;
						color:rgb(147,153,159);
					}
				}
				.ele-buy{
					position:absolute;
					bottom:0;
					right:0;
				}
			}
		}
	}
}
</style>