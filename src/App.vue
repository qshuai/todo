<template>
	<el-main>
		<NewTask></NewTask>
		<br>
		<TimeSelector></TimeSelector>
	</el-main>
</template>

<script>
	import TimeSelector from './components/TimeSelector.vue'
	import NewTask from './components/NewTask.vue'

	export default {
		name: 'App',
		components: {
			TimeSelector,
			NewTask,
		}
	}
</script>

<style>
	#app {
		width: 1000px;
		height: 100%;
		margin: 0 auto;
	}
</style>
