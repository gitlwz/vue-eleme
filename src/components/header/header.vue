<template>
	<div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img width="64" height="64" src="http://static.galileo.xiaojukeji.com/static/tms/seller_avatar_256px.jpg" />
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				</div>
				<div class="description">
					{{seller.description}}/{{seller.deliveryTime}}分钟送达
				</div>
				<div class="support">
					<span class="decrease"></span>
					<span class="text">{{ seller.supports && seller.supports[0].description}}</span>
				</div>
				<div class="supports-count" @click.stop.prevent="showDetail">
					<span class="text">{{ seller.supports && seller.supports.length}}个</span>
					<span class="supports-count-decrease">›</span>
				</div>
			</div>
		</div>
		<div class="bulletin-wrapper" @click.stop.prevent="showDetail">
			<span class="bulletin-title"></span>
			<span class="bulletin-text">{{seller.bulletin}}</span>
		</div>
		<div class="background">
			<img :src="seller.avatar" />
		</div>
		<transition name="fade">
			<div v-show="detailShow" class="transition-detail" @click.stop.prevent="hideDetail">
				<div class="detail-main">
					<h1>{{seller.name}}</h1>
					<div class="star-wrapper">
						<star :score='2.5'></star>
					</div>
					<div class="title">
						<div class="line"></div>
						<div class="text">优惠信息</div>
						<div class="line"></div>
					</div>
					<ul class="supports">
						<li v-for="(item, index) in seller.supports">
							<span class="icon decrease" :class="item.type|supportsClass"></span>
							<span class="text">{{item.description}}</span>
						</li>
					</ul>
					<div class="title">
						<div class="line"></div>
						<div class="text">商家公告</div>
						<div class="line"></div>
					</div>
					<div class="introduced">
						<p>
							{{seller.bulletin}}
						</p>
					</div>
				</div>
				<div class="detail-clos">
					×
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
	import star from '../star/star'
	require('./header.less')
	const imgClasss = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
	export default {
		name: 'header',
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				detailShow: false,

			}
		},
		created() {
			
		},
		components: {
			star: star
		},
		filters: {
			supportsClass(value) {
				return imgClasss[value]
			}
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