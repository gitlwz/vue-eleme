<template>
	<div class="lwz-content">
		<div class="goods">
			<div class="leaft-menu" ref="menuWrapper">
				<ul>
					<li class="item" v-for="(item, index) in goods" :class="{'current':currentIndex === index}" @click="selectMenu(index, $event)">
						<span class="text">
           					<span v-show="item.type>0" class="icon"  :class="item.type|supportsClass"></span>{{item.name}}
						</span>
					</li>
				</ul>
			</div>
			<div class="right-content" ref="foodWrapper">
				<ul>
					<li class="group group-list-hook" v-for="item in goods">
						<h1 class="group-title">{{item.name}}</h1>
						<ul>
							<li class="group-item" v-for="food in item.foods">
								<div class="group-icon" @click.stop.prevent="toggleFood($event,food)">
									<img :src="food.icon" width="57" />
								</div>
								<div class="group-content">
									<h2 class="title">{{food.name}}</h2>
									<p class="desc">{{food.description}}</p>
									<div class="extra">
										<span class="count">月售{{food.sellCount}}</span><span>好评{{food.rating}}</span>
									</div>
									<div class="price">
										<span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
									</div>
									<div class="cartControl-wrapper">
										<cartcontrol :food.sync="food" @increment="incrementTotal"></cartcontrol>
									</div>
								</div>
							</li>
						</ul>
					</li>
				</ul>
			</div>

		</div>
		<div class="card">
			<shopcart :foods.sync="getFood" :min-price="seller.minPrice" ref="shopCart"></shopcart>
		</div>
		<food :food="food" ref="food"></food>
	</div>
</template>

<script>
	import BScroll from 'better-scroll';
	import data from '@/assets/data.json';
	import cartcontrol from '@/components/cartcontrol/cartcontrol';
	import shopcart from '@/components/shopcart/shopcart';
	import food from '@/components/food/food';
	import './goods.less';
	const imgClasss = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
	export default {
		name: 'goods',
		data() {
			return {
				food:{},
				seller: {},
				goods: [],
				scrolly: 0,
				listHeight: []
			}
		},
		components: {
			cartcontrol: cartcontrol,
			shopcart: shopcart,
			food: food
		},
		created() {
			this.goods = data.goods;
			this.seller = data.seller;
			this.$nextTick(() => {
				this._initScroll();
				this._calculateHeight();
			});
		},
		computed: {
			currentIndex() {
				for(let i = 0; i < this.listHeight.length; i++) {
					let height = this.listHeight[i];
					let height2 = this.listHeight[i + 1];
					if(!height2 || (this.scrolly >= height && this.scrolly < height2)) {
						return i;
					}
				}
				return 0;
			},
			getFood() {
				let foods = [];
				this.goods.forEach((good) => {
					good.foods.forEach((food) => {
						if(food.count) {
							foods.push(food);
						}
					});
				});
				return foods
			}
		},
		filters: {
			supportsClass(value) {
				return imgClasss[value]
			}
		},
		methods: {
			_initScroll() {
				this.menuScroll = new BScroll(this.$refs.menuWrapper, {
					click: true
				});
				this.foodScroll = new BScroll(this.$refs.foodWrapper, {
					probeType: 3,
					click: true
				});
				this.foodScroll.on('scroll', (pos) => {
					this.scrolly = Math.abs(Math.round(pos.y));
				});
			},
			_calculateHeight() {
				let foodList = this.$refs.foodWrapper.getElementsByClassName('group-list-hook');
				let height = 0;
				this.listHeight.push(height);
				for(let i = 0; i < foodList.length; i++) {
					let item = foodList[i];
					height += item.clientHeight;
					this.listHeight.push(height);
				}
			},
			toggleFood(event,food) {
				if(!event._constructed) {
					// 去掉自带click事件的点击
					return;
				}
				this.food = food;
				this.$refs.food.toggleShow()
			},
			selectMenu(index, event) {
				if(!event._constructed) {
					// 去掉自带click事件的点击
					return;
				}
				let foodList = this.$refs.foodWrapper.getElementsByClassName('group-list-hook');
				let el = foodList[index];
				this.foodScroll.scrollToElement(el, 300);
			},
			incrementTotal(event) {
				this.$refs.shopCart.drop(event)
			}
		}

	}
</script>

<style>

</style>