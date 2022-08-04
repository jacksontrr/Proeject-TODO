<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TasksProgress :progress="progress" />
		<NewTask @taskAdded="addTask" />
		<TaskGrid :tasks="tasks" @taskStateChanged="toggleTaskState" @taskDeleted="deleteTask" />
	</div>
</template>

<script>
import NewTask from './components/NewTask.vue'
import TaskGrid from './components/TaskGrid.vue'
import TasksProgress from './components/TasksProgress.vue'

export default {
	components: {
		TaskGrid, NewTask, TasksProgress
	},
	data() {
		return {
			tasks: []
		}
	},
	computed: {
		progress() {
			const total = this.tasks.length
			const completed = this.tasks.filter(task => !task.pending).length
			return Math.round(completed / total * 100) || 0
		}
	},
	watch: {
		tasks: {
			handler() {
				localStorage.setItem('tasks', JSON.stringify(this.tasks))
			},
			deep: true
		}
	},
	methods: {
		addTask(task) {
			const sameName = t => t.name === task.name
			const reallyNew = this.tasks.filter(sameName).length == 0 
			if (reallyNew && task.name != '') {
				this.tasks.push({
					name: task.name,
					pending: task.peding || true
				})
			}
		},
		deleteTask(i){
			this.tasks.splice(i, 1)
		},
		toggleTaskState(i){
			this.tasks[i].pending = !this.tasks[i].pending
		}
		/*deleteTask(task){
		* 	const i = this.tasks.indexOf(task)
		*	if (i > -1) {
		* 		this.tasks.splice(i, 1)
		*	}
		* }
		*/
	},
	created() {
		const json = localStorage.getItem('tasks')
		const arrayTasks = JSON.parse(json)
		if (Array.isArray(arrayTasks)) {
			this.tasks = JSON.parse(tasks) 
		}else{
			this.tasks = []
		}
	}
}
</script>

<style>
body {
	font-family: 'Lato', sans-serif;
	background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
	color: #FFF;
}

#app {
	display: flex;
	flex: 1;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	height: 100vh;
}

#app h1 {
	margin-bottom: 5px;
	font-weight: 300;
	font-size: 3rem;
}
</style>
