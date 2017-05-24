<template>
	<div class="ratingselect">
		<div class="rating-type">
			<span class="block positive" @click="select(2, $event)" :class="{active:selectType===2}">1</span>
			<span class="block positive" @click="select(0, $event)" :class="{active:selectType===0}">2</span>
			<span class="block negative" @click="select(1, $event)" :class="{active:selectType===1}">3</span>
		</div>
	</div>
</template>

<script>
	import "./ratingselect.less"
	export default {
		name: 'ratingselect',
		props: {
			indexType: {
				type: Number,
				default: 2
			},
		},
		data() {
			return {
				selectType: 2
			}
		},
		created(){
			this.selectType = this.indexType;
		},
		methods: {
			select(index, event) {
				if(index === this.selectType) return;
				this.selectType = index;
				this.$emit('increment', 'selectType', index);
			}
		}
	}
</script>

<style>

</style>