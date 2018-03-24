<template>
	<div>
		<div class="stack-wrapper">
			<stack ref="stack" :pages="pages" ></stack>
		</div>
	</div>
</template>
<script type="text/ecmascript-6">
import Stack from '../common/stack' 
export default {
	data() {
		return {
			pages: []
		}
	},
	mounted() {		
		setTimeout(() => {
			this.pages = [
				{
					src: 'static/img/1.png',
					alt: "01"
				},
				{
					src: 'static/img/2.png',
					alt: "02"
				},
				{
					src: 'static/img/3.png',
					alt: "03"
				},
				{
					src: 'static/img/4.png',
					alt: "04"
				},
				{
					src: 'static/img/5.png',
					alt: "05"
				},
				{
					src: 'static/img/6.png',
					alt: "06"
				},
				{
					src: 'static/img/7.png', 
					alt: "07" 
				}
			]
		},2000)	
	},
	methods:{},
	components:{
		Stack
	}
}
</script>
<style scoped lang="stylus" rel="stylesheet/stylus">
	.stack-wrapper 
		width: 320px
		height:320px
		margin: 0 auto
		padding: 0
		position: relative
		pointer-events:none
		list-style:none
</style>