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
		console.log(selectedTask);

		let desiredTaskToDelete = tasks.findIndex(
			(item) => item.id == selectedTask.id
		);

		tasks.splice(desiredTaskToDelete, 1);
		tasks = tasks;
	};
</script>

<style>
</style>

<main>
	<form
		on:submit={(e) => {
			event_createTask(e, document.getElementById('input-task-name').value);
		}}>
		<input id="input-task-name" type="text" required />
		<button>Create Task</button>
	</form>

	<div>
		{#each tasks as task (task.id)}
			<div>
				<p>{task.name}</p>
				<button
					on:click={() => {
						event_changeStatus(task.id);
					}}>
					{#if task.status == false}mark complete{:else}completed{/if}
				</button>
				<button
					on:click={() => {
						event_deleteTask(task);
					}}>delete</button>
			</div>
		{:else}
			<div>
				<p>You have no tasks. Create some above.</p>
			</div>
		{/each}
	</div>
</main>
