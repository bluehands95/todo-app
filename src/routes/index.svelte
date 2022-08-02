<script>
	// variables
	let state = false;
	let todoTitle = '';
	let todoDesc = '';
	let todos = [
		{ title: 'Learn Svelte', description: 'Learn 4 pomodoro sessions of Svelte', completed: false },
		{
			title: 'Learn Javascript',
			description: 'Learn 4 pomodoro sessions of Svelte',
			completed: false
		},
		{ title: 'Code Projects', description: 'Deploy this todo list project', completed: false }
	];

	// functions
	function handleCancel() {
		if (todoTitle || todoDesc) {
			return;
		}
		state = false;
	}
	function addTodo() {
		if (todoDesc && todoTitle) {
			let newTodo = {
				title: todoTitle,
				description: todoDesc,
				completed: false
			};
			todos = [...todos, newTodo];
			todoDesc = '';
			todoTitle = '';
			state = false;
			return;
		}
		alert('No description or title added');
	}
	function markTodo({ todo }) {
		if (todo.completed) {
			let amsure = confirm('Are you sure?');
			if (amsure) {
				todo.completed = false;
				todo = todo;
				todos = [...todos];
			}
			return;
		}
		todo.completed = true;
		todo = todo;
		todos = [...todos];
	}

	// efects
	import { fly } from 'svelte/transition';
</script>

<div class="flex flex-wrap justify-between bg-white m-4 p-3">
	<p class="text-stone-900 text-2xl">Todo List</p>
	<a href="https://github.com/bluehands95.com">
		<img
			src="https://logos-download.com/wp-content/uploads/2016/09/GitHub_logo.png"
			alt="github-logo"
			class="w-7"
		/>
	</a>
</div>

<form class="flex m-4" on:submit|preventDefault={addTodo}>
	<div class="m-4">
		<input
			bind:value={todoTitle}
			on:focus={() => (state = true)}
			on:blur={handleCancel}
			class="p-2 border-solid border-2"
			type="text"
			placeholder="Write todo"
		/>
		{#if state}
			<input
				in:fly={{ y: 50, duration: 400 }}
				out:fly={{ y: 50, duration: 400 }}
				bind:value={todoDesc}
				class="p-2 border-2"
				type="text"
				placeholder="Insert title"
			/>
			<button />
		{/if}
	</div>
</form>

<div
	class="grid gap-1 2xl:grid-cols-5 xl:grid-cols-4 md:grid-cols-3 sm:grid-cols-2 grid-cols-1 justify-center m-4"
>
	{#each todos as todo}
		<div
			on:click={markTodo({ todo })}
			class="p-6 rounded-lg shadow-lg bg-white max-w-sm h-auto m-1 cursor-pointer"
		>
			<h1 class:line-through={todo.completed} class="text-lg p-1">{todo.title}</h1>
			<p class:line-through={todo.completed} class="text-m text-slate-600 p-1">
				{todo.description}
			</p>
			{#if todo.completed}
				<p class="text-m text-green-600 p-1">Completed</p>
			{:else}
				<p class="text-m text-red-600 p-1">Incompleted</p>
			{/if}
		</div>
	{/each}
</div>
