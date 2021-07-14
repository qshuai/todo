<template>
	<el-form label-width="80px" class="task-detail">
		<el-form-item label="任务名称">
			<el-input v-model="title"></el-input>
		</el-form-item>
		<el-form-item label="分类">
			<el-select v-model="category" placeholder="请输入任务分类">
				<el-option v-for="(category,index) in categories" :label="category.name" :value="category.id"
					:key="index"></el-option>
			</el-select>
		</el-form-item>
		<el-form-item label="截止时间">
			<el-col :span="11">
				<el-date-picker type="datetime" placeholder="选择截止日期" v-model="deadline" style="width: 100%;"
					format="YYYY-MM-DD HH:mm:ss" value-format="YYYY-MM-DD HH:mm:ss">
				</el-date-picker>
			</el-col>
		</el-form-item>
		<el-form-item label="JIRA编号">
			<el-input v-model="jira"></el-input>
		</el-form-item>
		<el-form-item label="wiki链接">
			<el-input v-model="wiki"></el-input>
		</el-form-item>
		<el-form-item label="涉及项目">
			<el-col>
				<el-tag class="project" :key="project" v-for="project in projects" closable :disable-transitions="false"
					@close="handleClose(project)">
					{{project}}
				</el-tag>
				<el-input v-if="addProject" v-model="inputValue" ref="saveTagInput" @keyup.enter="handleInputConfirm"
					@blur="handleInputConfirm" style="width: 150px; margin-right: 5px;" size="small">
				</el-input>
				<el-button class="button-new-tag" size="small" @click="showInput">+ New Project</el-button>
			</el-col>
		</el-form-item>
		<el-form-item label="分支名称">
			<el-col>
				<el-input v-model="branch"></el-input>
			</el-col>
		</el-form-item>
		<el-form-item label="子任务">
			<el-col v-for="subTask in subTasks" :key="subTask.id" style="width: 100%;">
				<el-checkbox class="subTask" checked="false">{{subTask.title}}</el-checkbox>
			</el-col>
			<el-input v-model="newSubTask" @keyup.enter="addSubTask" @blur="addSubTask"></el-input>
		</el-form-item>
		<el-form-item label="备注">
			<el-input type="textarea" v-model="remark" autosize></el-input>
		</el-form-item>
		<el-form-item>
			<el-button type="primary" @click="onSubmit">提 交</el-button>
		</el-form-item>
	</el-form>
</template>

<script>
	import axios from 'axios';
	import config from '@/components/Config';

	export default {
		name: 'TodoDetail',
		props: ["detail"],
		data() {
			return {
				id: this.detail.id,
				title: this.detail.title,
				category: 0,
				categories: [],
				deadline: this.detail.deadline,
				jira: this.detail.jira,
				wiki: this.detail.wiki,
				projects: this.detail.projects,
				branch: this.detail.branch,
				subTasks: [],
				remark: this.detail.remark,
				newSubTask: "",
				addProject: false,
				inputValue: '',
			}
		},
		created() {
			// 构造页面时请求数据
			axios.get(config.host + '/categories')
				.then((response) => {
					if (response.data.data.length <= 0) {
						return;
					}

					this.categories = response.data.data;
				}).catch((error) => {
					this.$notify({
						title: "任务分类",
						message: "获取任务分类数据错误: " + error
					});
				});

			this.category = this.detail.category;
			this.subTasks = this.detail.sub_tasks;
		},
		methods: {
			onSubmit() {
				axios.put(config.host + '/task', {
					id: this.id,
					title: this.title,
					category: this.category,
					deadline: this.deadline,
					jira: this.jira,
					wiki: this.wiki,
					projects: this.projects,
					branch: this.branch,
					subtasks: this.subTasks,
					remark: this.remark,
				}).then(function(response) {
					console.log(response);
				}).catch(function(error) {
					console.log(error);
				});
			},
			handleClose(project) {
				this.projects.splice(this.projects.indexOf(project), 1);
			},

			showInput() {
				this.addProject = true;
				this.$nextTick(() => {
					this.$refs.saveTagInput.$refs.input.focus();
				});
			},

			handleInputConfirm() {
				let inputValue = this.inputValue;
				if (inputValue) {
					this.projects.push(this.inputValue);
				}
				this.inputValue = '';
			},
			addSubTask() {
				let newSubTask = this.newSubTask;
				if (newSubTask) {
					this.subTasks.push({
						title: newSubTask,
						state: 0
					});
				}
				this.newSubTask = "";
			}
		}
	}
</script>

<style>
	.task-detail {
		border-left: 1px solid #DAE3F2;
		border-right: 1px solid #DAE3F2;
		padding: 5px;
	}

	.project {
		margin-right: 5px;
	}

	.subTask.checked {}
</style>
