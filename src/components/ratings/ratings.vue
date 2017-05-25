<template>
	<div class="ratings lwz-content" ref="ratingsel">
		<div class="ratings-content">
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
				<ratingselect @increment="incrementTotal" :only-content="onlyContent" :ratings="ratings" :selectType="selectType"></ratingselect>
			</div>
			<div class="bottom">
				<ul class="bottom-content">
					<li class="li-item" v-for="rating in ratings" v-show="needShow(rating.rateType, rating.text)">
						<div class="item-left">
							<img :src="rating.avatar" width="28" height="28" />
						</div>
						<div class="item-right">
							<h1 class="name">{{rating.username}}</h1>
							<div class="star-wrappers">
								<div class="star-group">
									<star :score='rating.score' startSize='min-size'></star>
								</div>
								<span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
							</div>
							<p class="text">
								{{rating.text}}
							</p>
							<div class="recommend" v-show="rating.recommend &&rating.recommend.length">
								<i class="iconfont icon-damuzhi"></i>
								<span class="genre" v-for="item in rating.recommend">{{item}}</span>
							</div>
							<div class="time">
								{{rating.rateTime | formatDate}}
							</div>
						</div>
					</li>
				</ul>
			</div>

		</div>
	</div>
</template>

<script>
	import data from '@/assets/data.json';
	import { formatDate } from '@/assets/utility/date.js'
	import star from '../star/star';
	import BScroll from 'better-scroll';
	import ratingselect from '@/components/ratingselect/ratingselect';
	import './ratings.less';
	const SELECT = {
		ALL: 2,
		SATIS: 0,
		TAUNT: 1
	}
	export default {
		name: 'ratings',
		data() {
			return {
				selectType: SELECT.ALL,
				onlyContent: false,
				ratings: [],
				seller: {}
			}
		},
		created() {
			this.ratings = data.ratings;
			this.seller = data.seller;
			this.$nextTick(() => {
				this.scroll = new BScroll(this.$refs.ratingsel, {
					click: true
				});
			});
		},
		computed: {

		},
		components: {
			star: star,
			ratingselect
		},
		filters: {
			formatDate(time) {
				let date = new Date(time);
				return formatDate(date, 'yyyy-MM-dd hh:mm');
			}
		},
		methods: {
			incrementTotal(selectType, type) {
				this[selectType] = type;
				this.$nextTick(() => {
					this.scroll.refresh();
				});
			},
			needShow(type, text) {
				if(this.onlyContent && !text) {
					return false;
				}
				if(this.selectType === 2) {
					return true;
				} else {
					return type === this.selectType;
				}
			}
		}
	}
</script>
<style>

</style>