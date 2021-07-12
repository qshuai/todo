<template>
	<el-row class="new-task" :gutter="25">
		<el-col :span="18">
			<el-input placeholder="请输入待办任务" suffix-icon="el-icon-plus" v-model="input1"></el-input>
		</el-col>
		<el-col :span="6">
			<el-date-picker placeholder="选择截止日期" v-model="value1" type="date" :disabled-date="disabledDate"
				:shortcuts="shortcuts"></el-date-picker>
		</el-col>
	</el-row>
</template>

<script>
	export default {
		name: 'NewTask',
		props: {
			msg: String
		},
		data() {
			return {
				input1: "",
				disabledDate(time) {
					return time.getTime() > Date.now()
				},
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
				},{
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
				},{
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
				},{
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
				},{
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
				},{
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
				},{
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
				value1: '',
				value2: '',
			};
		}
	}
</script>

<style>
	.new-task {
		margin-top: 20px;
		margin-right: 0px;
	}
</style>
