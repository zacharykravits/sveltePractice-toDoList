<script>
	import { each } from "svelte/internal";

	// data
	let tasks = [
		{ id: 0, name: "example task", dueDate: new Date(), status: false },
	];

	let currentIdCount = 0;

	const event_createTask = (e, enteredTaskName, enteredTaskDate) => {
		e.preventDefault();
		//
		console.log(enteredTaskName);
		console.log(enteredTaskDate);
		//
		const taskToBePushed = new Object();
		taskToBePushed.id = currentIdCount += 1;
		taskToBePushed.name = enteredTaskName;
		taskToBePushed.dueDate = enteredTaskDate;
		taskToBePushed.status = false;

		console.log(currentIdCount);

		tasks.push(taskToBePushed);

		tasks = tasks;

		console.log(tasks);
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
</script>

<style>
</style>

<main>
	<form
		on:submit={(e) => {
			event_createTask(e, document.getElementById('input-task-name').value, document.getElementById('input-task-date').value);
		}}>
		<input id="input-task-name" type="text" required />
		<input id="input-task-date" type="date" required />
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
			</div>
		{:else}
			<div>
				<p>You have no tasks. Create some above.</p>
			</div>
		{/each}
	</div>
</main>
