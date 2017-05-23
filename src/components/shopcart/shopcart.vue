<template>
	<div class="shopcart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{enough:totalCount>0}">
						<i class="iconfont icon-gouwuche" :class="{enough:totalCount>0}"></i>
					</div>
					<div v-show ='totalCount>0' class="num">{{totalCount}}</div>
				</div>
				<div class="prie" :class="{enough:totalPrice > 0}">
					￥{{this.totalPrice}}
				</div>
			</div>
			<div class="content-right">
				<div class="pay " :class="{enough:totalPrice >= minPrice}">
					{{payDesc}}
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'shopcart',
		props: {
			minPrice: {
				type: Number,
				default: 0
			}
		},
		data() {
			return {
				totalCount: 0,
				totalPrice: 0
			}
		},
		computed: {
			payDesc(){
				if(this.totalCount === 0){
					return `￥${this.minPrice}元起送`
				}else if(this.totalPrice < this.minPrice){
					let diff = this.minPrice - this.totalPrice
					return `还差￥${diff}元起送`
				}else{
					return `去结算`
				}
			}
		},
		methods: {
			dropAndMoney(event, price) {
				if(price > 0) {
					this.totalCount++;
					this.totalPrice += price
				} else if(price < 0) {
					this.totalCount--;
					this.totalPrice += price
				}
			}
		}
	}
</script>

<style lang="less" rel="stylesheet/less">
	@import url("shopcart.less");
</style>