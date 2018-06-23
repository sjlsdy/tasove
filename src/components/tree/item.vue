<template>
	<ul class="tree" :class="{root:data[0] && data[0]['level'] == 1}">
		<li v-for="item in data">
			<p>
				<Button type="ghost" icon="plus-round" size="small" @click="add(item)" style="margin-right: 10px;"></Button><Button type="ghost" icon="edit" size="small" @click="edit(item)"></Button><span v-for="o in level(item.level)"></span>{{item.name}}&nbsp;&nbsp;&nbsp;&nbsp;（{{item.child.length}}条子数据）
			</p>
			<item v-if="item.child.length > 0" :data="item.child" @on-add="add($event)" @on-edit="edit($event)"></item>
		</li>
	</ul>
</template>

<script>
	import item from './item'
	export default {
		name: 'item',
		components: {
			item
		},
		props: ['data'],
		methods: {
			level(num) {
				let arr = [];
				for(let i = 1; i < parseInt(num); i++) {
					arr.push('<i></i>');
				}
				return arr
			},
			add(item) {
				this.$emit("on-add", item);
			},
			edit(item) {
				this.$emit("on-edit", item);
			},
		}
	}
</script>

<style>
	.root {
		border: #DDDDDD 1px solid;
		border-top: none;
	}
	
	.tree {
		line-height: 40px;
	}
	
	.tree li {
		list-style: none;
		border-top: #DDD 1px solid;
	}
	
	.tree li button {
		margin-right: 10px;
	}
	
	.tree li p {
		padding-left: 1em;
	}
	
	.tree li p:hover {
		background: #F9F9F9;
	}
	
	.tree li p span {
		width: 2em;
		display: inline-block;
	}
</style>