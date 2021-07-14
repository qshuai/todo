<template>
	<el-tabs type="border-card" @tab-click="switchTab">
		<el-tab-pane value="0">
			<template #label>
				<span><i class="el-icon-date"></i> 今天</span>
			</template>
			<TodoItem v-for="(item,index) in entries" :key=index :item="item"></TodoItem>
		</el-tab-pane>
		<el-tab-pane label="明天" :lazy="true">
			<TodoItem v-for="(item,index) in entries" :key=index :item="item"></TodoItem>
		</el-tab-pane>
		<el-tab-pane label="本周" :lazy="true">
			<TodoItem v-for="(item,index) in entries" :key=index :item="item"></TodoItem>
		</el-tab-pane>
		<el-tab-pane label="全部" :lazy="true">
			<TodoItem v-for="(item,index) in entries" :key=index :item="item"></TodoItem>
		</el-tab-pane>
	</el-tabs>
</template>

<script>
	import TodoItem from './TodoItem.vue'
	import moment from 'moment';
	import axios from 'axios';
	import config from '@/components/Config';

	export default {
		name: 'TimeSelector',
		data() {
			return {
				startTime: moment().format("YYYY-MM-DD"),
				endTime: moment(new Date().getTime() + 24 * 3600 * 1000).format("YYYY-MM-DD"),
				entries: [],
				preIndex: 0,
			}
		},
		components: { // 注册组件
			'TodoItem': TodoItem
		},
		created() {
			this.loadData();
		},
		methods: {
			switchTab(tab) {
				let index = tab.index;
				if (this.preIndex == index) {
					// 点击两次不在重新请求
					return;
				}
				this.preIndex = index;

				const date = new Date();
				if (index == 0) {
					this.startTime = moment().format("YYYY-MM-DD");
					this.endTime = moment(new Date().getTime() + 24 * 3600 * 1000).format("YYYY-MM-DD");
				} else if (index == 1) {
					this.startTime = moment(new Date().getTime() + 24 * 3600 * 1000).format("YYYY-MM-DD");
					this.endTime = moment(new Date().getTime() + 48 * 3600 * 1000).format("YYYY-MM-DD");
				} else if (index == 2) {
					this.startTime = moment(date.getTime() - (date.getDay() - 1) * 24 * 3600 * 1000).format("YYYY-MM-DD");
					this.endTime = moment(date.getTime() + (8 - date.getDay()) * 24 * 3600 * 1000).format("YYYY-MM-DD");
				} else if (index == 3) {
					this.startTime = "";
					this.endTime = "";
				}

				this.loadData();
			},
			loadData() {
				axios.get(config.host + "/tasks", {
					params: {
						start_time: this.startTime,
						end_time: this.endTime,
					}
				}).then((response) => {
					this.entries = [];
					let data = response.data.data.entries;
					let len = data.length;
					if (len <= 0) {
						this.entries.splice(0);
						return;
					}

					let i = 0;
					for (; i < len; i++) {
						this.entries.push(data[i]);
					}
				}).catch((error) => {
					this.$notify({
						title: "任务列表",
						message: error
					});
					console.log(error);
				})
			}
		}
	}
</script>

<stype>

</stype>
