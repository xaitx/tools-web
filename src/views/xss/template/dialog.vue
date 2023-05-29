<template>
	<div class="system-dic-dialog-container">
		<el-dialog :title="state.dialog.title" v-model="state.dialog.isShowDialog" width="769px">
			<el-form ref="dicDialogFormRef" :model="state.ruleForm" size="default" label-width="90px" disabled="true">
				<el-row :gutter="35">
					<el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24" class="mb20">
						<el-form-item label="时间">
							<el-input v-model="state.infoData.time" placeholder="时间" clearable></el-input>
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24" class="mb20">
						<el-form-item label="IP">
							<el-input v-model="state.infoData.ip" placeholder="IP" clearable></el-input>
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24" class="mb20">
						<el-form-item label="来源">
							<el-input v-model="state.infoData.sorce" placeholder="来源" clearable></el-input>
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24" class="mb20">
						<el-form-item label="客户端">
							<el-input v-model="state.infoData.client" placeholder="客户端" clearable></el-input>
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24" class="mb20">
						<el-form-item label="请求">
							<el-input v-model="state.infoData.request" placeholder="请求" clearable></el-input>
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24" class="mb20">
						<el-form-item label="数据">
							<el-input v-model="state.infoData.data" type="textarea" placeholder="数据" rows="5"></el-input>
						</el-form-item>
					</el-col>
				</el-row>
			</el-form>
			<template #footer>
				<span class="dialog-footer">
					<el-button type="primary" @click="closeDialog" size="default">关闭</el-button>
				</span>
			</template>
		</el-dialog>
	</div>
</template>

<script setup name="systemDicDialog">
import { reactive, ref } from 'vue';


// 定义变量内容
const dicDialogFormRef = ref();
const state = reactive({
	infoData: {
		time: '2023-12-16 12:12:12',
		ip: '127.0.0.1',
		source: '局域网',
		client: 'Windows',
		request: 'POST',
		data: '{"cookie":""}',
	},
	dialog: {
		isShowDialog: false,
		title: '详情信息'
	},
});

// 打开弹窗
const openDialog = (type,row) => {
	state.dialog.isShowDialog = true;
};
// 关闭弹窗
const closeDialog = () => {
	state.dialog.isShowDialog = false;
};
// 暴露变量
defineExpose({
	openDialog,
});
</script>
