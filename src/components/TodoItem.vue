<template>
	<el-row v-if="!deleted" class="radius todo-item" justify="left" align="middle" v-on:click="showDetail">
		<el-col :span="0.5">
			<i v-show="state == 0" class="el-icon-loading icon" @click.stop="completeTask(item.id)"></i>
		</el-col>
		<el-col :span="0.5">
			<i v-show="state == 1" class="el-icon-paperclip icon done-color" @click.stop="redoTask(item.id)"></i>
		</el-col>
		<el-col :span="17">
			<div v-show="state == 0" class="doing">{{item.title}}</div>
			<div v-show="state == 1" class="done done-color">{{item.title}}</div>
		</el-col>
		<el-col :span="5">
			<div class="deadline">{{item.deadline}}</div>
		</el-col>
		<el :span="0.5">
			<i class="el-icon-delete delete" @click.stop="deleteTask(item.id)"></i>
		</el>
	</el-row>
	<TodoDetail class="todo-detail" v-if="visible" :detail="item"></TodoDetail>
</template>

<script>
	import TodoDetail from './TodoDetail.vue';
	import axios from 'axios';
	import config from '@/components/Config';

	export default {
		name: 'TodoItem',
		props: ["item"],
		components: { // 注册组件
			'TodoDetail': TodoDetail
		},
		data() {
			return {
				visible: false,
				state: 0,
				deleted: false,
			}
		},
		created() {
			this.state = this.item.state;
		},
		methods: {
			showDetail() {
				this.visible = !this.visible
			},
			completeTask(id) {
				axios.put(config.host + "/task/complete/" + id)
					.then(() => {
						this.state = 1;
					}).catch((error) => {
						this.$notify({
							title: "任务状态",
							message: "设置任务完成失败，请稍后重试: " + error,
						});
					});
			},
			redoTask(id) {
				axios.put(config.host + "/task/redo/" + id)
					.then(() => {
						this.state = 0;
					}).catch((error) => {
						this.$notify({
							title: "任务状态",
							message: "设置任务未完成失败，请稍后重试: " + error,
						});
					});
			},
			deleteTask(id) {
				this.$confirm('此操作将永久删除该任务, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					axios.delete(config.host + "/task/" + id)
						.then(() => {
							this.deleted = true;
							this.$message({
								type: "success",
								message: "删除成功",
							});
						}).catch((error) => {
							this.$notify({
								title: "任务操作",
								message: "删除该任务失败，请稍后重试: " + error,
							});
						});
				});
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

	.deadline {
		color: #8C7853;
	}

	.icon {
		margin: 10px 8px;
	}

	.icon:hover {
		cursor: pointer;
	}

	.done {
		text-decoration: line-through;
	}

	.done-color {
		color: darkgray;
	}

	.delete {
		margin: 10px 5px;
	}

	.delete:hover {
		cursor: pointer;
	}
</style>
