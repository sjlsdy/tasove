<template>
	<div>
		{{data}}=={{type}}
		<Form :model="formItem" :label-width="80">
			<FormItem label="父级ID">
				<Input v-model="formItem.parentId" placeholder="" :disabled="true"></Input>
			</FormItem>
			<FormItem label="父级">
				<span>{{formItem.parentName}}</span>
				<Select v-model="formItem.parentName" style="width:100%;">
					<Option v-for="item in cityList" :value="item.value" :key="item.value">{{ item.label }}</Option>
				</Select>
			</FormItem>
			<FormItem label="ID">
				<Input v-model="formItem.id" placeholder="" :disabled="true"></Input>
			</FormItem>
			<FormItem label="等级">
				<Input v-model="formItem.level" placeholder="" :disabled="true"></Input>
			</FormItem>
			<FormItem label="名称">
				<Input v-model="formItem.name" placeholder=""></Input>
			</FormItem>
			<FormItem label="别称">
				<Input v-model="formItem.aliasName" placeholder=""></Input>
			</FormItem>
			<FormItem label="">
				<Button type="primary" :loading="loading" @click="ok" v-if="type === 'add'">新增</Button>
				<Button type="primary" :loading="loading" @click="update" v-if="type === 'edit'">更新</Button>
			</FormItem>
		</Form>
	</div>
</template>

<script>
	export default {
		components: {},
		props: ['type', 'data'],
		data() {
			return {
				loading: false,
				formItem: {
					parentId: this.data.parentId,
					parentName: this.data.parentName,
					id: this.data.id,
					level: this.type === 'add' ? parseInt(this.data.level) + 1 : this.data.level,
					name: this.data.name,
					aliasName: this.data.aliasName,
				},
				cityList: [{
					value: 'New York',
					label: 'New York'
				}, ]
			}
		},
		methods: {
			ok() {
				let _self = this;
				_self.loading = true;
				this.$http({
					method: 'post',
					data: {
						'name': _self.formItem.name,
						'aliasName': _self.formItem.aliasName,
						'parentId': _self.formItem.parentId,
						'level': _self.formItem.level,
					},
					url: '/index.php?m=admin&c=region&a=add'
				}).then(function(res) {
					_self.loading = false;
					if(res.data.status === 0) {
						_self.$Message.success(res.data.message);
						_self.$emit('on-submit');
					} else {
						_self.$Message.error(res.data.message);
					}
				}).catch(res => {
					_self.loading = false;
					_self.$Message.error('请求失败：' + res);
				});
			},
			update() {
				let _self = this;
				_self.loading = true;
				this.$http({
					method: 'post',
					data: {
						'id': _self.formItem.id,
						'name': _self.formItem.name,
						'aliasName': _self.formItem.aliasName,
						'parentId': _self.formItem.parentId,
						'level': _self.formItem.level,
					},
					url: '/index.php?m=admin&c=region&a=edit'
				}).then(function(res) {
					_self.loading = false;
					if(res.data.status === 0) {
						_self.$Message.success(res.data.message);
						_self.$emit('on-submit');
					} else {
						_self.$Message.error(res.data.message);
					}
				}).catch(res => {
					_self.loading = false;
					_self.$Message.error('请求失败：' + res);
				});
			},
		}
	}
</script>

<style>

</style>