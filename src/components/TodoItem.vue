<template>
	<el-row class="radius todo-item" justify="left" align="middle" v-on:click="showDetail">
		<el-col :span="21">
			<div class="grid-content bg-purple">这里是任务标题</div>
		</el-col>
		<el-col :span="3">
			<div class="grid-content bg-purple-light">2021-09-09</div>
		</el-col>
	</el-row>
	<TodoDetail class="todo-detail" v-if="visible"></TodoDetail>
</template>

<script>
	import TodoDetail from './TodoDetail.vue'

	export default {
		name: 'TodoItem',
		components: { // 注册组件
			'TodoDetail': TodoDetail
		},
		data() {
			return {
				visible: true,
			}
		},
		methods: {
			showDetail() {
				this.visible = !this.visible
			}
		}
	}
</script>

<style>
	.todo-item {
		margin-top: 5px;
		width: 100%;
		height: 50px;
		padding: 1px 0 0 5px;
	}

	.radius {
		border: 1px solid lightgray;
		border-radius: 4px
	}

	.radius:hover {
		border-color: yellowgreen;
	}

	.todo-detail {
		margin-top: 5px;
	}
</style>
