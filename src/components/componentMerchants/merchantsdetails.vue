<template>
	<div>
		{{author}}
		11111
	</div>
</template>

<script>
	export default{
		name:"detail",
		data(){
			return {
				name:"商家详情"
			}
		},
//		props:["clickname"]
		computed:{
			author(){
				return this.$store.state.author
			}
		},
		methods:{// 定义事件
			
		}
	}
</script>

<style>
	
</style>