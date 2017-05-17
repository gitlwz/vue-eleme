<template>
	<div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img width="64" height="64" src="http://static.galileo.xiaojukeji.com/static/tms/seller_avatar_256px.jpg" />
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">粥品香坊（回龙观）</span>
				</div>
				<div class="description">
					蜂鸟专送/38分钟送达
				</div>
				<div class="support">
					<span class="decrease"></span>
					<span class="text">在线支付满28减5</span>
				</div>
				<div class="supports-count">
					<span class="text">5个</span>
					<span class="supports-count-decrease">›</span>
				</div>
			</div>
		</div>
		<div class="bulletin-wrapper" @click="showDetail">
			<span class="bulletin-title"></span>
			<span class="bulletin-text">粥品香坊其烹饪粥料的秘方源于中国千年古法，在融和现代制作工艺，由世界烹饪大师屈浩先生领衔研发。坚守纯天然、0添加的良心品质深得消费者青睐，发展至今成为粥类的引领品牌。是2008年奥运会和2013年园博会指定餐饮服务商。</span>
		</div>
		<div class="background">
			<img src="http://static.galileo.xiaojukeji.com/static/tms/seller_avatar_256px.jpg" />
		</div>
		<transition name="fade">
			<div v-if="detailShow" class="transition-detail" @click="hideDetail">
				<div class="detail-main">
					<h1>粥品香坊（回龙观）</h1>
					<div class="star-wrapper">
						<star :score='1.5'></star>
					</div>
					<div class="title">
						<div class="line"></div>
						<div class="text">优惠信息</div>
						<div class="line"></div>
					</div>
					<ul class="supports">
						<li>
							<span class="icon decrease"></span>
							<span class="text">在线支付满28减5</span>
						</li>
					</ul>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
	import star from '../star/star'
	require('./header.less')
	export default {
		name: 'header',
		porps:{
			seller:{
				type:Object
			}
		},
		data() {
			return {
				detailShow: false
			}
		},
		components: {
			star: star
		},
		methods: {
			showDetail() {
				this.detailShow = true;
			},
			hideDetail() {
				this.detailShow = false;
			}
		}
	}
</script>