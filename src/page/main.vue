<template>
	<div class="layout">
		<Layout>
			<Header>
				<Menu mode="horizontal" theme="dark" active-name="1">
					<div class="layout-logo"></div>
					<div class="layout-nav">
						<MenuItem name="1">
						<Icon type="ios-navigate"></Icon>
						Item 1
						</MenuItem>
						<MenuItem name="2">
						<Icon type="ios-keypad"></Icon>
						Item 2
						</MenuItem>
						<MenuItem name="3">
						<Icon type="ios-analytics"></Icon>
						Item 3
						</MenuItem>
						<MenuItem name="4">
						<Icon type="ios-paper"></Icon>
						Item 4
						</MenuItem>
					</div>
				</Menu>
			</Header>
			<Layout>
				<Sider hide-trigger :style="{background: '#fff'}">
					<Menu active-name="1-2" theme="light" width="auto" :open-names="['1']">
						<Submenu name="1">
							<template slot="title">
								<Icon type="ios-navigate"></Icon>
								Item 1
							</template>
							<MenuItem name="1-1">Option 1</MenuItem>
							<MenuItem name="1-2">Option 2</MenuItem>
							<MenuItem name="1-3">Option 3</MenuItem>
						</Submenu>
						<Submenu name="2">
							<template slot="title">
								<Icon type="ios-keypad"></Icon>
								Item 2
							</template>
							<MenuItem name="2-1">Option 1</MenuItem>
							<MenuItem name="2-2">Option 2</MenuItem>
						</Submenu>
						<Submenu name="3">
							<template slot="title">
								<Icon type="ios-analytics"></Icon>
								Item 3
							</template>
							<MenuItem name="3-1">Option 1</MenuItem>
							<MenuItem name="3-2">Option 2</MenuItem>
						</Submenu>
					</Menu>
				</Sider>
				<Layout :style="{padding: '0 24px 24px'}">
					<Breadcrumb :style="{margin: '24px 0'}">
						<BreadcrumbItem>Home</BreadcrumbItem>
						<BreadcrumbItem>Components</BreadcrumbItem>
						<BreadcrumbItem>Layout</BreadcrumbItem>
					</Breadcrumb>
					<Content :style="{padding: '24px', minHeight: '280px', background: '#fff'}">
						<h2>地区列表</h2>
						<br />
						<Row>
							<Col span="24">
							<tree :data="regionData" @on-add="addRegion($event)" @on-edit="editRegion($event)"></tree>
							</Col>
						</Row>
						<br /> 总共{{regionNum}}条数据=={{editData}}==
						<Modal v-model="addRegionModal" :title="addRegionTitle">
							<add-region :type="regionAddOrEdit" :data='editData' @on-submit="ok" v-if="addRegionModal"></add-region>
							<div slot="footer"></div>
						</Modal>
					</Content>
				</Layout>
			</Layout>
		</Layout>
	</div>
</template>

<script>
	import tree from '../components/tree/item'
	import addRegion from '../components/addRegion/index'
	export default {
		components: {
			tree,
			addRegion
		},
		data() {
			return {
				regionNum: [],
				regionData: [],
				regionAddOrEdit: '',
				addRegionModal: false,
				editData: {},
			}
		},
		computed: {
			addRegionTitle() {
				if(this.regionAddOrEdit === 'add') {
					return '新增地点'
				} else if(this.regionAddOrEdit === 'edit') {
					return '编辑地点'
				}
			}
		},
		mounted() {
			this.initData();
		},
		methods: {
			addRegion(e) {
				this.regionAddOrEdit = 'add';
				this.addRegionModal = true;
				this.editData = {
					parentId: e.id,
					parentName: e.name,
					level: e.level,
				};
			},
			editRegion(e) {
				this.regionAddOrEdit = 'edit';
				this.addRegionModal = true;
				this.editData = e;
			},
			ok() {
				this.addRegionModal = false;
				this.initData();
			},
			initData() {
				let _self = this;
				_self.data1 = [];
				this.$http({
					method: 'get',
					url: '/index.php?m=admin&c=region&a=thelist'
				}).then(function(res) {
					if(res.data.status === 0) {
						_self.regionNum = res.data.data.length;
						_self.$Message.success(res.data.message);
						let data = [];
						// 先将所有根节点遍历出来
						res.data.data.filter((item) => {
							item['child'] = [];
							if(item.level == 1 && item.parentId == 0) {
								data.unshift(item);
							}
						});
						// 再将节点塞进父级的children
						res.data.data.map((item) => {
							res.data.data.filter((childitem) => {
								if(childitem['id'] === item['parentId']) {
									childitem['child'].unshift(item);
								}
							});
						});
						_self.regionData = data;
					} else {
						_self.$Message.error(res.data.message);
					}
				}).catch(res => {
					_self.$Message.error('请求失败：' + res);
				});
			},
		},
	}
</script>

<style>

</style>