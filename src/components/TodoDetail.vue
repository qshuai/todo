<template>
	<el-form label-width="80px" class="task-detail">
		<el-form-item label="任务名称">
			<el-input v-model="title"></el-input>
		</el-form-item>
		<el-form-item label="分类">
			<el-select v-model="category" placeholder="请输入任务分类">
				<el-option label="区域一" value="shanghai"></el-option>
				<el-option label="区域二" value="beijing"></el-option>
			</el-select>
		</el-form-item>
		<el-form-item label="截止时间">
			<el-col :span="11">
				<el-date-picker type="date" placeholder="选择截止日期" v-model="deadline" style="width: 100%;">
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
				<el-button class="button-new-tag" size="small" @click="showInput">+ New Tag</el-button>
			</el-col>
		</el-form-item>
		<el-form-item label="分支名称">
			<el-col>
				<el-input v-model="branch"></el-input>
			</el-col>
		</el-form-item>
		<el-form-item label="子任务">
			<el-col v-for="subTask in subTasks" :key="subTask.name" style="width: 100%;">
				<el-checkbox class="subTask" checked="subTask.completed">{{subTask.name}}</el-checkbox>
			</el-col>
			<el-input v-model="newSubTask" @keyup.enter="addSubTask" @blur="addSubTask"></el-input>
		</el-form-item>
		<el-form-item label="备注">
			<el-input type="textarea" v-model="desc" autosize></el-input>
		</el-form-item>
		<el-form-item>
			<el-button type="primary" @click="onSubmit">提 交</el-button>
		</el-form-item>
	</el-form>
</template>

<script>
	export default {
		name: 'TodoDetail',
		data() {
			return {
				title: "",
				category: [],
				deadline: "",
				branch: "",
				type: 1,
				resource: "",
				desc: "",
				a: "",
				projects: ["smart-sale-service", "customer-dispatch"],
				addProject: false,
				inputValue: '',
				subTasks: [{
						name: "任务1",
						completed: false
					},
					{
						name: "任务2",
						completed: false
					},
					{
						name: "任务3",
						completed: true
					},
				],
				newSubTask: "",
			}
		},
		methods: {
			onSubmit() {
				console.log(this.$axios);
				this.$axios.post('/user', {
					firstName: 'Fred',
					lastName: 'Flintstone'
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
						name: newSubTask,
						completed: false
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
