<template>
	<div :class="`task ${!task.status ? 'is-complete' : ''}`">
		<div class="content">{{ task.todo_name }}</div>
		<div class="buttons">
			<button @click="toggleDone">{{ task.status ? 'Undo' : 'Done' }}</button>
			<button @click="removeTask" class="delete">Delete</button>
		</div>
	</div>
</template>

<script>
export default {
	props: ['task'],
	methods: {
		toggleDone() {
			this.$store.commit('TOGGLE_TASK', this.task);
		},
		removeTask() {
			this.$store.commit('REMOVE_TASK', this.task);
		}
	}
}

</script>

<style>

</style>