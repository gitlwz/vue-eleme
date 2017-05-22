<template>
	<div class="cartcontrol">
		<transition name="fadeINOUT" enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
			<div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart($event)">
				<transition name="bounce" enter-active-class="animated bounceInRight" leave-active-class="animated bounceOutRight">
					<span class="font font-del">-</span>
				</transition>
			</div>
		</transition>
		<span class="cart-count" v-show="food.count > 0 ">
      		{{food.count}}
    	</span>
		<span class="font font-add" @click.stop.prevent="addCart($event)">＋</span>
	</div>
</template>

<script>
	export default {
		name: 'cartcontrol',
		props: ['food'],
		data() {
			return {}
		},
		created() {

		},
		methods: {
			addCart(event) {
				if(!event._constructed) {
					// 去掉自带click事件的点击
					return;
				}
				if(!this.food.count) {
					this.$set(this.food, 'count', 1);
				} else {
					this.food.count++;
				}
			}
		}
	}
</script>

<style lang="less" rel="stylesheet/less">
	@import "cartcontrol.less";
</style>