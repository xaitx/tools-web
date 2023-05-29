<template>
	<div class="system-dic-container layout-padding">
		<el-card shadow="hover" class="layout-padding-auto">
			<div class="system-user-search mb15">
				<el-input size="default" placeholder="请输入Cookie内容" style="max-width: 180px"> </el-input>
				<el-button size="default" type="primary" class="ml10">
					<el-icon>
						<ele-Search />
					</el-icon>
					查询
				</el-button>
			</div>
			<el-table :data="state.tableData.data" v-loading="state.tableData.loading" style="width: 100%" @row-dblclick="onOpenInfo">
				<el-table-column prop="id" label="序号" width="50" />
				<el-table-column prop="name" label="项目名称" show-overflow-tooltip></el-table-column>
				<el-table-column prop="introduction" label="简介" show-overflow-tooltip></el-table-column>
				<el-table-column prop="state" label="状态" show-overflow-tooltip></el-table-column>
				<el-table-column prop="createTime" label="创建时间" show-overflow-tooltip></el-table-column>
				<el-table-column
					label="Tag"
					width="100"
				>
					<template #default="scope">
						<el-tag :type="scope.row.state == 1?'success':''" disable-transitions>{{ scope.row.state == 1?'开启':'关闭' }}</el-tag>
					</template>
				</el-table-column>
				<el-table-column label="操作" width="100">
					<template #default="scope">
						<el-button size="small" text type="primary" @click="onRowDel(scope.row)">修改</el-button>
						<el-button size="small" text type="primary" @click="onRowDel(scope.row)">删除</el-button>
					</template>
				</el-table-column>
			</el-table>
			<el-pagination
				@size-change="onHandleSizeChange"
				@current-change="onHandleCurrentChange"
				class="mt15"
				:pager-count="5"
				:page-sizes="[10, 20, 30]"
				v-model:current-page="state.tableData.param.pageNum"
				background
				v-model:page-size="state.tableData.param.pageSize"
				layout="total, sizes, prev, pager, next, jumper"
				:total="state.tableData.total"
			>
			</el-pagination>
		</el-card>
		<DicDialog ref="dicDialogRef" @refresh="getTableData()" />
	</div>
</template>

<script setup name="systemDic">
import { defineAsyncComponent, reactive, onMounted, ref } from 'vue';
import { ElMessageBox, ElMessage } from 'element-plus';

// 引入组件
const DicDialog = defineAsyncComponent(() => import('/@/views/xss/list/dialog.vue'));

// 定义变量内容
const dicDialogRef = ref();
const state = reactive({
	tableData: {
		data: [],
		total: 0,
		loading: false,
		param: {
			pageNum: 1,
			pageSize: 10,
		},
	},
});

// 初始化表格数据
const getTableData = () => {
	state.tableData.loading = true;
	const data = [];
	// for (let i = 0; i < 2; i++) {
	// 	data.push({
	// 		dicName: i === 0 ? '角色标识' : '用户性别',
	// 		fieldName: i === 0 ? 'SYS_ROLE' : 'SYS_UERINFO',
	// 		describe: i === 0 ? '这是角色字典' : '这是用户性别字典',
	// 		status: true,
	// 		createTime: new Date().toLocaleString(),
	// 		list: [],
	// 	});
	// }
	for (let i = 0; i < 10; i++) {
		data.push({
			id: i,
			name: 'xss',
			introduction: '测试',
			state: i%2,
			createTime: '2020',
		});
	}
	state.tableData.data = data;
	state.tableData.total = state.tableData.data.length;
	setTimeout(() => {
		state.tableData.loading = false;
	}, 500);
};

// 打开详情窗口
const onOpenInfo = (row) => {
	dicDialogRef.value.openDialog(row);
	console.log(row);
};

// 删除字典
const onRowDel = (row) => {
	ElMessageBox.confirm(`此操作将永久删除是否继续?`, '提示', {
		confirmButtonText: '确认',
		cancelButtonText: '取消',
		type: 'warning',
	})
		.then(() => {
			getTableData();
			ElMessage.success('删除成功');
		})
		.catch(() => {});
};
// 分页改变
const onHandleSizeChange = (val) => {
	state.tableData.param.pageSize = val;
	getTableData();
};
// 分页改变
const onHandleCurrentChange = (val) => {
	state.tableData.param.pageNum = val;
	getTableData();
};
// 页面加载时
onMounted(() => {
	getTableData();
});
</script>
