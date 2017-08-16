<template lang="html">
	<div class="goods">
		<div class="goods-menu"  id='goods-menu'>
			<ul>
				<li v-for="(item,index) in goods"  :key="index"   :class="{active:index==current}" @click="selectCategory(index)">
					<p>{{item.name}}</p>
				</li>
			</ul>
		</div>
		<div class="goods-list" id="goods-list">
			<div ref="list">
				<dl  v-for="(item,index) in goods" :key="index" ref="index">
					<dt>{{item.name}}</dt>
					<dd v-for="(food,idx) in item.foods" :key="idx" @click="selectFood(food)">
						<img :src="food.icon" alt="">
						<div class="food">
							<h2>{{food.name}}</h2>
							<p class="desc">{{food.description}}</p>
							<p class="sale">月售{{food.sellCount}}份 好评率{{food.rating}}</p>
							<p class="price">
								<strong><span>￥</span>{{food.price}}</strong>
								<del v-if="food.oldPrice">￥{{food.oldPrice}}</del>
							</p>
							<div class="ele-buy">
								<ele-buy :food="food"></ele-buy>
							</div>
						</div>
					</dd>
				</dl>
			</div>
		</div>
		<transition name="slide">
			<ele-food v-show="show" @close="show=false" :food="currentFood"></ele-food>
		</transition>
		<div class="ele-cart">
			<ele-cart :cartGoods="cartGoods" :minPrice="seller.minPrice" :deliveryPrice="seller.deliveryPrice"></ele-cart>
		</div>
	</div>
</template>

<script>
const shop=require('../../api/data.json')

import IScoll from '../../../static/js/iscroll-probe'
import Buy from '../Buy/Buy'
import Food from '../Food/Food'
import Cart from '../Cart/Cart'

export default {
	data(){
		return {
			goods:[],//该商家所有的外卖商品
			current:0,//表示当前所在的分类，默认为0，表示第一个分类
			listHeight:[],//表示右侧商品分类容器的高度
			show : false , //是否显示food组件，默认不显示
			currentFood:{},//表示当前选中的商品
			seller:{},//表示商家的信息
		}
	},
	created(){
		this.goods=shop.goods;
		this.seller=shop.seller;
	},
	methods:{
		selectFood(food){
			this.show=true;
			this.currentFood=food;
		},
		selectCategory(index){
			this.current=index;
			let dls=this.$refs.list.getElementsByTagName('dl');
			this.listScoll.scrollToElement(dls[index]);
			// this.listScoll.scrollToElement(this.$refs.index);
		},
		_getListHeight(){
			let dls=this.$refs.list.getElementsByTagName('dl');
			let arr=[0];
			for(let i=0;i<dls.length;i++){
				arr.push(dls[i].clientHeight+arr[i]);
			}
			// this.listHeight=arr;
			return arr;
		}
	},
	mounted(){
		this.menuScoll=new IScoll('#goods-menu',{click:true});
		this.listScoll=new IScoll('#goods-list',{probeType:2,click:true});
		//调用_getListHeight()获取商品列表容器的高度
		this.listHeight=this._getListHeight();
		// console.log(this.listHeight)

		let that=this;
		//给this.listScoll注册scoll事件
		this.listScoll.on('scroll',function(){
			// console.log(this.y)
			let dis=Math.abs(this.y);
			for(let i=0;i<that.listHeight.length;i++){
				let start=that.listHeight[i];
				let end=that.listHeight[i+1];
				if((dis>=start && dis<end) || !end){
					that.current=i;
					console.log(that.current)
					break;
				}
			}
		});
	},
	components:{
		'ele-buy':Buy,
		'ele-food':Food,
		'ele-cart':Cart
	},
	computed:{
		cartGoods(){
			let cart=[];
			this.goods.forEach(item=>{
				item.foods.forEach(food=>{
					if(food.count){
						cart.push(food);
					}
				});
			});
			console.log(cart)
			return cart;
		}
	}
}
</script>

<style lang="less" scoped>
.slide-enter,.slide-leave-active{transform:translateX(100%);opacity:0;}
.slide-enter-active,.slide-leave-active{transition:all 0.5s;}
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