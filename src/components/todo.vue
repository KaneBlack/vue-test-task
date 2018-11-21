<template>
	<div class="todo">
		<h1 class="title">Checklist</h1>
		<ui-tabs type="text" class="todo-list">
			<ui-tab title="pending">
				<todo-list :tasks="tasks" :completedItem="pending"></todo-list>
			</ui-tab>

			<ui-tab title="complete">
				<todo-list :tasks="tasks" :completedItem="completed"></todo-list>
			</ui-tab>
		</ui-tabs>

		<div class="ui-elements">
			<ui-textbox placeholder="e.g. 'read vue.js guide'" v-model="newTaskName"
						@keydown-enter="addTask" :class="{error : this.taskIsEmpty}"></ui-textbox>
			<ui-button color="primary" @click="addTask" icon="add">Add</ui-button>
		</div>
	</div>
</template>

<script>
    import TodoList from "./TodoList";
    import dataList from "./dataList";

    export default {
        components: {TodoList},
        data() {
            return {
                newTaskName: '',
                taskIsEmpty: false,
                tasks: dataList
            }
        },

        mounted() {
            if(localStorage.getItem('tasks')) {
                try {
                    this.tasks = JSON.parse(localStorage.getItem('tasks'));
                } catch(e) {
                    localStorage.removeItem('tasks');
                }
            }
        },

        methods: {
            addTask() {
                if (this.newTaskName.trim().length > 0) {
                    this.tasks.push({name: this.newTaskName, complete: false});
                    this.newTaskName = '';
                    this.saveTask();
                    this.taskIsEmpty = false;
                } else {
                    this.taskIsEmpty = true;
                }
            },
            saveTask() {
                let parsed = JSON.stringify(this.tasks);
                localStorage.setItem('tasks', parsed);
            },
            completed: function (taskList) {
                return taskList.filter(function (taskItem) {
                    return taskItem.complete
                })
            },
            pending: function (taskList) {
                return taskList.filter(function (taskItem) {
                    return !taskItem.complete
                })
            },
        }
    };
</script>

<style lang="scss">
	.todo {
		max-width: 500px;
		width: 100%;
		margin: auto;
		background: #fff;
		padding: 20px;
		border-radius: 5px;
		box-shadow: rgba(0, 0, 0, 0.3) 3px 3px 15px;

		.title {
			margin-top: 0;
		}

		.tasks {
			list-style: none;
			padding: 0;
			height:200px;
			overflow: auto;
		}
		&-list {
		}
	}

	.ui-elements {
		display: flex;

	}

	.ui-textbox {
		flex: 1 1 auto;
		margin-right: 20px;
		&.error {
			input {
				border-bottom-width: 2px;
				border-bottom-color: rgba(#f00, .3);
			}
		}
	}

	.ui-button {
		flex: 0 0 auto;
	}

	.task-item {
		overflow: hidden;
		&.complete {
			.ui-checkbox__label-text {
				text-decoration: line-through;
			}
		}
	}
</style>
