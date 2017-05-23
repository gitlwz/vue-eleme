<template>
	<div id="app">
		<v-header :seller="seller"></v-header>
		<div class="tab border-1px bottom-border">
			<div class="tab-item">
				<router-link to="/goods">
					商品
				</router-link>
			</div>
			<div class="tab-item">
				<router-link to="/ratings">
					评论
				</router-link>
			</div>
			<div class="tab-item">
				<router-link to="/seller">
					商家
				</router-link>
			</div>
		</div>
		<transition name="custom-classes-transition" :enter-active-class="transitionEnterName" :leave-active-class="transitionLeaveName">
			<keep-alive>
				<router-view></router-view>
			</keep-alive>
		</transition>
	</div>
</template>

<script>
	import header from '@/components/header/header';
	import data from '@/assets/data.json';
	require('./index.less');
	export default {
		name: 'app',
		data() {
			return {
				seller: {},
				transitionEnterName: 'animated bounceInRight',
				transitionLeaveName: 'animated bounceOutLeft'
			}
		},
		components: {
			"v-header": header
		},
		created() {
			setTimeout(function() {
				this.seller = data.seller
			}.bind(this), 1000)

		},
		watch: {
			'$route' (to, from) {

			}
		}
	}
</script>

<style scoped>
	.content {
		position: relative;
		overflow: hidden;
	}
	
	.tab {
		position: relative;
		width: 100%;
		height: 40px;
		display: flex;
		line-height: 40px;
	}
	
	.tab-item {
		flex: 1;
		text-align: center;
	}
	
	.tab .tab-item a {
		display: block;
		font-size: 14px;
		color: rgb(77, 85, 93);
	}
	
	.tab .tab-item .router-link-active {
		color: rgb(240, 20, 20);
	}
</style>