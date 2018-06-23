<template>
	<div class="hello">
		<button @click="send">发送AJAX请求</button>
	</div>
</template>

<script>
	import tree from './tree/item'
	export default {
		name: 'HelloWorld',
		components: {
			tree
		},
		data() {
			return {
				msg: 'Welcome to Your Vue.js App'
			}
		},
		methods: {
			send() {
				this.$http({
					method: 'get',
					url: '/index.php?m=admin&c=region&a=thelist'
				}).then(function(resp) {
					console.log(resp.data);
				}).catch(resp => {
					console.log('请求失败：' + resp.status + ',' + resp.statusText);
				});
			},
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	h1,
	h2 {
		font-weight: normal;
	}
	
	ul {
		list-style-type: none;
		padding: 0;
	}
	
	li {
		display: inline-block;
		margin: 0 10px;
	}
	
	a {
		color: #42b983;
	}
</style>