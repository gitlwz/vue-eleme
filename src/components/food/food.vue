<template>
	<transition name="food-mask" enter-active-class="animated bounceInRight" leave-active-class="animated bounceOutRight">
		<div v-show="show" class="food" ref="reffood">
			<div>
				<div class="content">
					<div class="header">
						<img :src="food.image" />
						<div class="back" @click.stop.prevent="toggleShow">
							<i class="iconfont icon-weibiaoti6-copy"></i>
						</div>
					</div>
					<div class="referral">
						<h1 class="title">{{food.name}}</h1>
						<div class="detail">
							<span>月售{{food.sellCount}}份</span>
							<span> 好评率{{food.rating}}%</span>
						</div>
						<div class="price">
							<span class="now">￥{{food.price}}</span>
							<span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
						</div>
						<transition name="food-buy" enter-active-class="animated flipInX" leave-active-class="animated flipOutX">
							<div class="buy" @click.stop.prevent="addFirst($event)" v-show="!food.count || food.count === 0">
								加入购物车
							</div>
						</transition>
						<div v-show="food.count && food.count !== 0" class="cartControl-wrapper">
							<cartcontrol :food.sync="food" @increment="incrementTotal"></cartcontrol>
						</div>
					</div>
				</div>
				<div class="split"></div>
				<div v-show="food.info">
					<div class="info">
						<h1>商品信息</h1>
						<p>{{food.info}}</p>
					</div>
					<div class="split"></div>
				</div>
				<div class="food-rating">
					<ratingselect @increment="incrementTotal" :only-content="onlyContent" :ratings="food.ratings" :selectType="selectType"></ratingselect>
				</div>
				<div class="bottom">
					<ul class="bottom-content">
						<li class="li-item" v-for="rating in food.ratings" v-show="needShow(rating.rateType, rating.text)">
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
	</transition>
</template>

<script>
	import BScroll from 'better-scroll';
	import cartcontrol from '@/components/cartcontrol/cartcontrol';
	import ratingselect from '@/components/ratingselect/ratingselect';
	import { formatDate } from '@/assets/utility/date.js';
	import star from '../star/star';
	export default {
		name: 'food',
		props: {
			food: {
				type: Object,
				default: {}
			}
		},
		data() {
			return {
				show: false,
				selectType: 2,
				onlyContent: false,
			}
		},
		created() {

		},
		filters: {
			formatDate(time) {
				let date = new Date(time);
				return formatDate(date, 'yyyy-MM-dd hh:mm');
			}
		},
		methods: {
			_reffood() {
				this.reffood = new BScroll(this.$refs.reffood, {
					click: true
				});
			},
			toggleShow() {
				this.show = !this.show;
				this.$nextTick(() => {
					this._reffood();
				});
			},
			incrementTotal(selectType, type) {
				this[selectType] = type;
				this.$nextTick(() => {
					this.reffood.refresh();
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
			},
			addFirst(event) {
				if(!event._constructed) {
					return;
				}
				this.$set(this.food, 'count', 1);
			},
			incrementTotal(event) {
				
			}
		},
		components: {
			ratingselect,
			star,
			cartcontrol
		}
	}
</script>

<style lang="less" rel="stylesheet/less">
	@import "food.less";
</style>