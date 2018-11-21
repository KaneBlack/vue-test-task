<template>
	<div>
		<transition-group tag="ul" name="list" class="tasks">
			<li v-for="task in completedItem(tasks)" v-bind:key="task.name" :class="{complete : task.complete}"
				class="task-item">
				<ui-checkbox v-model="task.complete">{{task.name}}</ui-checkbox>
			</li>
		</transition-group>
	</div>
</template>

<script>
    export default {
        name: "TodoList",
        props: {
            tasks: {
                type: Array,
            },
            completedItem: {
                type: Function,
            },
        },
    }
</script>

<style>

</style>
