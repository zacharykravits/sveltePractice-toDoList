<script>
	import { each } from "svelte/internal";

	// data
	let tasks = [
		{
			id: 0,
			name: "example task",
			status: false,
		},
	];

	let currentIdCount = 0;

	const event_createTask = (e, enteredTaskName) => {
		e.preventDefault();
		//
		console.log(enteredTaskName);
		//
		const taskToBePushed = new Object();
		taskToBePushed.id = currentIdCount += 1;
		taskToBePushed.name = enteredTaskName;
		taskToBePushed.status = false;

		console.log(currentIdCount);

		tasks.push(taskToBePushed);

		tasks = tasks;

		console.log(tasks);

		clearInput();
	};

	const clearInput = () => {
		document.getElementById("input-task-name").value = "";
	};

	const event_changeStatus = (selectedId) => {
		tasks.forEach((task) => {
			if (task.id == selectedId) {
				console.log(selectedId);
				console.log(task.status);
				task.status = !task.status;
				tasks = tasks;
			}
		});
	};

	const event_deleteTask = (selectedTask) => {
		let desiredTaskToDelete = tasks.findIndex(
			(item) => item.id == selectedTask.id
		);

		tasks.splice(desiredTaskToDelete, 1);
		tasks = tasks;
	};
</script>

<style>
	body,
	html {
		margin: 0;
		padding: 0;
	}

	p {
		font-family: Helvetica, sans-serif;
		padding: 0;
		margin: 0;
	}

	button {
		padding: 8px;
		border-radius: 4px;
		border: 1px solid #b4b4b4;
	}

	.delete-button {
		color: #646464;
		background: #ffffff;
		border: none;
	}

	.status-button-completed {
		width: 124px;
		padding: 8px;
		background: #93ffd6;
	}

	.status-button-needs-done {
		width: 124px;
		padding: 8px;
		background: #93ceff;
	}

	.form {
		padding: 16px 32px;
		background: #ffffff;
		border-bottom: 1px solid #cecece;
		width: 100%;
		position: fixed;
	}

	form > input {
		padding: 8px;
		min-width: 320px;
	}

	form > button {
		background: #93ceff;
	}

	.task {
		display: flex;
		align-items: center;
		justify-content: space-between;
		width: 100%;
		padding: 8px 0px;
		border-bottom: 1px solid #cecece;
	}

	.list {
		padding: 92px 32px 0px 32px;
		overflow-y: scroll;
	}

	.emptyList-message {
		margin-top: 56px;
	}

	.task-left {
		display: flex;
		align-items: center;
	}

	.task-left > button {
		margin-right: 16px;
	}
</style>

<main>
	<form
		class="form"
		on:submit={(e) => {
			event_createTask(e, document.getElementById('input-task-name').value);
		}}>
		<input id="input-task-name" type="text" required />
		<button>create task</button>
	</form>

	<div class="list">
		<h1>Tasks</h1>
		{#each tasks as task (task.id)}
			<div class="task">
				<div class="task-left">
					{#if task.status == false}
						<button
							class="status-button-needs-done"
							on:click={() => {
								event_changeStatus(task.id);
							}}>mark complete
						</button>
					{:else}
						<button
							class="status-button-completed"
							on:click={() => {
								event_changeStatus(task.id);
							}}>completed
						</button>
					{/if}
					<p>{task.name}</p>
				</div>

				<button
					class="delete-button"
					on:click={() => {
						event_deleteTask(task);
					}}>delete
				</button>
			</div>
		{:else}
			<div class="emptyList-message">
				<p>You have no tasks. Create some above.</p>
			</div>
		{/each}
	</div>
</main>
