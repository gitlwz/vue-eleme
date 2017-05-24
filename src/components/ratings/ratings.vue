<template>
	<div class="ratings lwz-content" ref="ratingsel">
		<div class="ratings-content" >
			<div class="overview">
				<div class="left">
					<h1 class="score">{{seller.score}}</h1>
					<div class="title">综合评分</div>
					<div class="rankRate">高于周边商家{{seller.rankRate}}%</div>
				</div>
				<div class="right">
					<div class="score-wrapper">
						<span class="title text">服务态度</span>
						<div class="grade">
							<star :score='seller.serviceScore' startSize='min-size'></star>
						</div>
						<span class="score text">{{seller.serviceScore}}</span>
					</div>
					<div class="score-wrapper">
						<span class="title text">商品评分</span>
						<div class="grade">
							<star :score='seller.foodScore' startSize='min-size'></star>
						</div>
						<span class="score text">{{seller.foodScore}}</span>
					</div>
					<div class="delivery-wrapper">
						<span class="title">送达时间</span>
						<span class="deliveryTime">{{seller.deliveryTime}}分钟</span>
					</div>
				</div>
			</div>
			<div class="split"></div>
			<div class="centre">
				<ratingselect @increment="incrementTotal" :indexType="indexType.ALL"></ratingselect>
			</div>

		</div>
	</div>
</template>

<script>
	import data from '@/assets/data.json';
	import star from '../star/star';
	import BScroll from 'better-scroll';
	import ratingselect from '@/components/ratingselect/ratingselect';
	import './ratings.less';
	const SELECT ={
		ALL:2,
		SATIS:0,
		TAUNT:1
	}
	export default {
		name: 'ratings',
		data() {
			return {
				indexType:SELECT,
				ratings: [],
				seller: {}
			}
		},
		created() {
			this.ratings = data.ratings;
			this.seller = data.seller;
			this.$nextTick(() => {
				console.log('$$$$$$',this.$el)
				this.scroll = new BScroll(this.$refs.ratingsel, {
					click: true
				});
			});
		},
		components: {
			star: star,
			ratingselect
		},
		methods:{
			incrementTotal(selectType, type){
				console.log('$$$$$$',selectType, type)
			}
		}
	}
</script>
<style>

</style>