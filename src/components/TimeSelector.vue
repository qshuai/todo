<template>
	<el-tabs type="border-card">
		<el-tab-pane>
			<template #label>
				<span><i class="el-icon-date"></i> 今天</span>
			</template>
			<TodoItem></TodoItem>
		</el-tab-pane>
		<el-tab-pane label="明天">
			<TodoItem></TodoItem>
		</el-tab-pane>
		<el-tab-pane label="本周">
			<TodoItem></TodoItem>
		</el-tab-pane>
		<el-tab-pane label="全部">
			<TodoItem></TodoItem>
		</el-tab-pane>
	</el-tabs>
</template>

<script>
	import TodoItem from './TodoItem.vue'

	export default {
		components: { // 注册组件
			'TodoItem': TodoItem
		}
	}
</script>

<stype>

</stype>
