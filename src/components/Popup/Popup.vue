<template lang="html">
	<div class="popup">
		<h1>{{seller.name}}</h1>
		<div class="discount">
			<h2>优惠信息</h2>
			<ul>
				<li v-for="(item,index) in seller.supports" :key="index">{{item.description}}</li>
			</ul>
		</div>
		<div class="bulletin">
			<h2>商家公告</h2>
			<p>{{seller.bulletin}}</p>
		</div>
		<div class="close" @click="close">X</div>
	</div>
</template>

<script>
export default {
	props:{
		seller:{
			type:Object,
			required:true
		},
		show:{
			type:Boolean,
			required:true
		}
	},
	methods:{
		close(){
			this.$emit('close');
		}
	}
}
</script>

<style lang="less" scoped>
	.popup{
		position:fixed;
		top:0;
		bottom:0;
		left:0;
		right:0;
		background: rgba(7,17,27,.8);
		padding:64px 36px 32px;
		color:#fff;
		z-index:1;
		h1{
			font-size:16px;
			font-weight:700;
			line-height:16px;
			text-align:center;
		}
		.discount,.bulletin{
			h2{
				font-size:14px;
				font-weight:700;
				line-height:14px;
				text-align:center;
				padding-top:28px;
				padding-bottom:24px;
			}
		}
		.discount{
			li{
				font-size:12px;
				font-weight:200;
				line-height:12px;
				padding:12px;
			}
		}
		.bulletin{
			font-size:12px;
			font-weight:200;
			line-height:24px;
		}
		.close{
			position:absolute;
			bottom:32px;
			left:50%;
			transform:translateX(-50%);
		}
	}
</style>