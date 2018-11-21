<template>
	<div>
		<transition-group tag="ul" name="list" class="tasks">
			<li v-for="task in completedItem(tasks)" v-bind:key="task.name" :class="{complete : task.complete}"
				class="task-item">
				<ui-checkbox color="none" v-model="task.complete">{{task.name}}</ui-checkbox>
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

<style lang="scss">

	.task-item {
		overflow: hidden;
		&.complete {
			.ui-checkbox__label-text {
				text-decoration: line-through;
			}
		}

		.ui-checkbox {
			&.is-checked {
				.ui-checkbox__checkmark::after {
					border-color: #000;
				}
			}
		}
	}

</style>
