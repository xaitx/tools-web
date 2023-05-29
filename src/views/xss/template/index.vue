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
                <el-button size="default" type="success" class="ml10" @click="addPage">
					<el-icon>
						<ele-FolderAdd />
					</el-icon>
					新增模板
				</el-button>
			</div>
			<el-table :data="state.tableData.data" v-loading="state.tableData.loading" style="width: 100%">
				<el-table-column prop="id" label="序号" width="50" />
				<el-table-column prop="name" label="模板名称" show-overflow-tooltip></el-table-column>
				<el-table-column prop="introduction" label="模板简介" show-overflow-tooltip></el-table-column>
				<el-table-column prop="createTime" label="创建时间" show-overflow-tooltip></el-table-column>
				<el-table-column label="操作" width="100">
					<template #default="scope">
						<el-button size="small" text type="primary" @click="updatePage(scope.row)">修改</el-button>
						<el-button size="small" text type="primary" @click="deletePage(scope.row)">删除</el-button>
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
const DicDialog = defineAsyncComponent(() => import('/@/views/xss/template/dialog.vue'));

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
	for (let i = 0; i < 10; i++) {
		data.push({
			id: i,
			name: 'cookie',
			introduction: '获取cookiesesssion',
			createTime: '2020',
		});
	}
	state.tableData.data = data;
	state.tableData.total = state.tableData.data.length;
	setTimeout(() => {
		state.tableData.loading = false;
	}, 500);
};


// 修改页面
const updatePage = (row)=>{
    dicDialogRef.value.openDialog("update",row)

}
// 删除页面
const deletePage = (row)=>{
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
}
// 添加页面
const addPage = (row)=>{
    dicDialogRef.value.openDialog("update",row)
}

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
