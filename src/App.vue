<template>
	<div id="app">
		<v-header></v-header>
		<div class="tab border-1px bottom-border">
			<div class="tab-item">
				<router-link v-bind:to="'/goods'">
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

		<router-view></router-view>
	</div>
</template>

<script>
	import header from '@/components/header/header';

	export default {
		name: 'app',
		components: {
			"v-header": header
		}
	}
</script>

<style scoped>
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