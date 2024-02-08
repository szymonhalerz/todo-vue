<template>
	<div class="todo">
		<div class="header">
			<h1>ToDo List</h1>
			<input
				v-on:keyup.enter="submitTask"
				v-model="task"
				type="text"
				class="todo-input"
				placeholder="Wpisz treść zadania..." />
			<button @click="submitTask()" class="btn-add">Add</button>
		</div>
		<div class="todolist">
			<h3>Lista zadań:</h3>
			<p class="error-info">{{ errorMsg }}</p>
			<p class="error-info">{{ checkTasksArr }}</p>
			<ul>
				<li v-for="(task, index) in tasks" :key="index">
					<span :class="{ completed: tasks[index].status === 'check' }">{{ task.name }}</span>
					<div class="tools">
						<button @click="checkTask(index)" class="complete"><i class="fas fa-check"></i></button>
						<button @click="editTask(index)" class="edit">EDIT</button>
						<button @click="deleteTask(index)" class="delete"><i class="fas fa-times"></i></button>
					</div>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
	setup() {
		const errorMsg = ref('')
		const task = ref('')
		const editedTask = ref(null)
		const tasks = ref([])

		const checkTasksArr = computed(function () {
			if (tasks.value.length === 0) {
				return 'Brak zadań na liście.'
			}
		})

		const submitTask = () => {
			if (task.value === '') {
				return (errorMsg.value = 'Wpisz treść zadania.')
			}

			if (editedTask.value === null) {
				tasks.value.push({
					name: task.value,
					status: 'to-do',
				})
			} else {
				tasks.value[editedTask.value].name = task.value
				editedTask.value = null
			}

			task.value = ''
			errorMsg.value = ''
		}

		const deleteTask = index => {
			tasks.value.splice(index, 1)
		}

		const checkTask = index => {
			if (tasks.value[index].status === 'check') {
				tasks.value[index].status = 'to-do'
			} else {
				tasks.value[index].status = 'check'
			}
		}

		const editTask = index => {
			task.value = tasks.value[index].name
			editedTask.value = index
		}

		return {
			errorMsg,
			task,
			editedTask,
			tasks,
			checkTasksArr,
			submitTask,
			deleteTask,
			checkTask,
			editTask,
		}
	},
}
</script>
