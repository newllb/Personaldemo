<template>
	<div>
		{{author}}
		4444
	</div>
</template>

<script>
	export default{
		name:"comments",
		data(){
			return {
				name:"评论"
			}
		},
//		props:['clickname']
		computed:{
			author(){
				return this.$store.state.author
			}
		}
	}
</script>

<style>
	
</style>