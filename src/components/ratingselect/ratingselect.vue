<template>
	<div class="ratingselect">
		<div class="rating-type">
			<span class="block positive" @click="select(2, $event)" :class="{active:selectType===2}">全部 {{ratings.length}}</span>
			<span class="block positive" @click="select(0, $event)" :class="{active:selectType===0}">满意 {{positives.length}}</span>
			<span class="block negative" @click="select(1, $event)" :class="{active:selectType===1}">吐槽 {{nagatives.length}}</span>
		</div>
		<div class="switch" @click="toggleContent( $event)" :class="{'on':onlyContent}">
			<i class="iconfont icon-gou"></i>
			<span class="text">只看有内容的评价</span>
		</div>
	</div>
</template>

<script>
	import "./ratingselect.less"
	const POSITIVE = 0;
	const NEGATIVE = 1;
	const ALL = 0;
	export default {
		name: 'ratingselect',
		props: {
			selectType: {
				type: Number,
				default: 2
			},
			ratings: {
				type: Array,
				default() {
					return [];
				}
			},
			onlyContent:{
				default:false
			}
		},
		data() {
			return {
				
			}
		},
		created() {
			
		},
		computed: {
			positives() {
				return this.ratings.filter((rating) => {
					return rating.rateType === POSITIVE;
				});
			},
			nagatives() {
				return this.ratings.filter((rating) => {
					return rating.rateType === NEGATIVE;
				});
			}
		},
		methods: {
			select(index, event) {
				if(!event._constructed) {
					return;
				}
				if(index === this.selectType) return;
				this.$emit('increment', 'selectType', index);
			},
			toggleContent(event) {
				if(!event._constructed) {
					return;
				}
				console.log('$$$$$',this.onlyContent)
				this.$emit('increment', 'onlyContent',!this.onlyContent);
			}
		}
	}
</script>

<style>

</style>