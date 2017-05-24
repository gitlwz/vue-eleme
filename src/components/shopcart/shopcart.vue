<template>
	<div class="shopcart">
		<div class="content">
			<div class="content-left" @click="toggleList($event)">
				<div class="logo-wrapper">
					<div class="logo" :class="{enough:totalCount>0}">
						<i class="iconfont icon-gouwuche" :class="{enough:totalCount>0}"></i>
					</div>
					<div v-show='totalCount>0' class="num">{{totalCount}}</div>
				</div>
				<div class="prie" :class="{enough:totalPrice > 0}">
					￥{{this.totalPrice}}
				</div>
			</div>
			<div class="content-right">
				<div class="pay" :class="{enough:totalPrice >= minPrice}" @click="pay">
					{{payDesc}}
				</div>
			</div>
		</div>
		<transition name="shopcart-trans">
			<div class="shopcart-list" v-show="listShow">
				<div class="list-header">
					<h1>购物车</h1><span class="list-clear" @click="empty($event)">清空</span>
				</div>
				<div class="list-content" ref="listContent">
					<ul>
						<li class="shopcart-food" v-for="(food,index) in foods">
							<span class="name">{{food.name}}</span>
							<div class="price">
								￥{{food.price * food.count}}
							</div>
							<div class="cartControl-wrapper">
								<cartcontrol :food.sync="food"></cartcontrol>
							</div>
						</li>
					</ul>
				</div>
			</div>
		</transition>
		<transition name="shopcart-mask" enter-active-class="animated zoomIn" leave-active-class="animated zoomOut">
			<div class="list-mask" v-show="listShow" @click="toggleList"></div>
		</transition>
	</div>
</template>

<script>
	import cartcontrol from '@/components/cartcontrol/cartcontrol';
	import header from '@/components/header/header';
	import BScroll from 'better-scroll';
	export default {
		name: 'shopcart',
		props: {
			minPrice: {
				type: Number,
				default: 0
			},
			foods: {
				type: Array,
				default() {
					return [{
						price: 0,
						count: 0
					}];
				}
			}
		},
		data() {
			return {
				fold: true
			}
		},
		created() {

		},
		computed: {
			totalPrice() {
				let total = 0;
				this.foods.forEach((food) => {
					total += food.price * food.count;
				});
				return total;
			},
			totalCount() {
				let count = 0;
				this.foods.forEach((food) => {
					count += food.count;
				});
				return count;
			},
			payDesc() {
				if(this.totalCount === 0) {
					return `￥${this.minPrice}元起送`
				} else if(this.totalPrice < this.minPrice) {
					let diff = this.minPrice - this.totalPrice
					return `还差￥${diff}元起送`
				} else {
					return `去结算`
				}
			},
			listShow() {
				if(!this.totalCount) {
					this.fold = true;
					return false;
				}
				let show = !this.fold;
				if(show) {
					this.$nextTick(() => {
						if(!this.scroll) {
							this.scroll = new BScroll(this.$refs.listContent, {
								click: true
							});
						} else {
							this.scroll.refresh();
						}
					});
				}
				return show
			}
		},
		methods: {
			drop(event) {

			},
			toggleList() {
				if(!this.totalCount) {
					return false;
				}
				this.fold = !this.fold;
			},
			empty() {
				this.foods.forEach((food) => {
					food.count = 0;
				});
			},
			pay(){
				if(this.totalPrice >= this.minPrice){
					alert(`需要支付${this.totalPrice}元`)
				}
			}
		},
		components: {
			cartcontrol: cartcontrol
		}
	}
</script>
<style lang="less" rel="stylesheet/less">
	@import url("shopcart.less");
</style>