<!-- Component base on directory /components/ppkc -->
<template>
	<div>
		<tedikaStrukturOrganisasi :path="path" :lists="lists"/>
	</div>
</template>

<script>
	export default{
		name: 'tedika-struktur-organisasi',
		layout: 'default',

		data(){
			return{
				path: this.$route.name
			}
		},

		async asyncData({$axios}){
			const lists = await $axios.$get('/web/struktur-organisasi')
			
			return {
				lists
			}
		}
	}
</script>