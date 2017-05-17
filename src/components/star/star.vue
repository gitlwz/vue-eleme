<template>
	<div class="star">
		<div class="star-items">
			<span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>
		</div>
	</div>
</template>
<script>
	require("./star.less");
	const LENGTH = 5;
	const CLS_ON = 'on';
	const CLS_HALF = 'half';
	const CLS_OFF = 'off';
	export default {
		name: 'star',
		props:['score'],
		data() {
			return {
			}
		},
		computed: {
			itemClasses() {
				let result = [];
				let scores = Math.floor(this.score * 2) / 2;
				let hasDecimal = scores % 1 !== 0;
				let integer = Math.floor(scores);
				for (let i = 0; i < integer; i++) {
		          result.push(CLS_ON);
		        }
		        if (hasDecimal) {
		          result.push(CLS_HALF);
		        }
		        while (result.length < LENGTH) {
		          result.push(CLS_OFF);
		        }
		        return result;
			}
		}
	}
</script>
<style>

</style>