<template>
	<el-row class="new-task" :gutter="25">
		<el-col :span="18">
			<el-input placeholder="请输入待办任务" suffix-icon="el-icon-plus" v-model="title" @keyup.enter="createTask"
				@blur="createTask">
			</el-input>
		</el-col>
		<el-col :span="6">
			<el-date-picker placeholder="选择截止日期" v-model="deadline" type="datetime" :shortcuts="shortcuts"
				format="YYYY-MM-DD HH:mm:ss" value-format="YYYY-MM-DD HH:mm:ss" @keyup.enter="createTask">
			</el-date-picker>
		</el-col>
	</el-row>
</template>

<script>
	import axios from 'axios';
	import moment from 'moment';
	import config from '@/components/Config';

	export default {
		name: 'NewTask',
		data() {
			return {
				title: "",
				deadline: "",
				shortcuts: [{
					text: '今天',
					value: new Date(),
				}, {
					text: '明天',
					value: (() => {
						const date = new Date()
						date.setTime(date.getTime() + 3600 * 1000 * 24)
						return date
					})(),
				}, {
					text: '下周一',
					value: (() => {
						const date = new Date()
						var offset = 8 - date.getDay()
						if (date.getDay() == 0) {
							offset = 1
						}
						date.setTime(date.getTime() + (offset) * (3600 * 1000 * 24))
						return date
					})(),
				}, {
					text: '下周二',
					value: (() => {
						const date = new Date()
						var offset = 8 - date.getDay() + 1
						if (date.getDay() == 0) {
							offset = 1
						}
						date.setTime(date.getTime() + (offset) * (3600 * 1000 * 24))
						return date
					})(),
				}, {
					text: '下周三',
					value: (() => {
						const date = new Date()
						var offset = 8 - date.getDay() + 2
						if (date.getDay() == 0) {
							offset = 1
						}
						date.setTime(date.getTime() + (offset) * (3600 * 1000 * 24))
						return date
					})(),
				}, {
					text: '下周四',
					value: (() => {
						const date = new Date()
						var offset = 8 - date.getDay() + 3
						if (date.getDay() == 0) {
							offset = 1
						}
						date.setTime(date.getTime() + (offset) * (3600 * 1000 * 24))
						return date
					})(),
				}, {
					text: '下周五',
					value: (() => {
						const date = new Date()
						var offset = 8 - date.getDay() + 4
						if (date.getDay() == 0) {
							offset = 1
						}
						date.setTime(date.getTime() + (offset) * (3600 * 1000 * 24))
						return date
					})(),
				}, {
					text: '下周六',
					value: (() => {
						const date = new Date()
						var offset = 8 - date.getDay() + 5
						if (date.getDay() == 0) {
							offset = 1
						}
						date.setTime(date.getTime() + (offset) * (3600 * 1000 * 24))
						return date
					})(),
				}, {
					text: '下周日',
					value: (() => {
						const date = new Date()
						var offset = 8 - date.getDay() + 6
						if (date.getDay() == 0) {
							offset = 1
						}
						date.setTime(date.getTime() + (offset) * (3600 * 1000 * 24))
						return date
					})(),
				}],
			};
		},
		methods: {
			createTask() {
				let title = this.title;
				let deadline = this.deadline;

				if (title.replace(/(^s*)|(s*$)/g, "").length == 0 || deadline.replace(/(^s*)|(s*$)/g, "").length == 0) {
					return
				}
				this.title = "";
				this.deadline = "";

				axios.post(config.host + "/task", {
					"title": title,
					"deadline": moment(deadline).format("YYYY-MM-DD HH:mm:ss")
				}).then(() => {
					this.$notify({
						title: "新建任务",
						message: '恭喜你，新建任务成功！',
						type: 'success',
						duration: 1000
					});
				}).catch((error) => {
					this.$notify({
						title: '新建任务',
						message: '新建任务失败！' + error,
						type: 'error',
						duration: 2000
					});
				});
			},
		}
	}
</script>

<style>
	.new-task {
		margin-top: 20px;
		margin-right: 0px;
	}
</style>
