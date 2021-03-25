<svelte:head>
	<title>Todo App</title>
	<meta name="title" content="Todo App" />
	<meta name="description" content="A todo app with localstorage data saving." />
</svelte:head>

<script>
	import TailwindCss from '$lib/TailwindCSS.svelte';

	let tasks = [];
	let task='';
	if (typeof window !== 'undefined'){
		let tasksFromStorage = localStorage.getItem('tasks');
		tasks = tasksFromStorage ? JSON.parse(tasksFromStorage): []
	}
	
	$: {
		if (typeof window !== 'undefined') {
			window.localStorage.setItem('tasks', JSON.stringify(tasks))
		}
	}


	function handleNewTask() {
		tasks = [...tasks, {title:task, done: false, edit: false}]
		task = '';
	}

	function handleClear() {
		if(typeof window !== undefined){
			tasks = [];
		}
	}

	function removeTask(title) {
		tasks = tasks.filter((task) => task.title != title )
	}

</script>
<TailwindCss />

<main>
	<h1>ToDo!</h1>

	<div class="mb-3 pt-0 w-2/4 m-auto">
		<label>
			<input type="text" on:change={handleNewTask} bind:value={task} placeholder="Enter new task" class="px-3 py-3 rounded text-md shadow outline-none focus:outline-none focus:shadow-outline w-2/3 bg-gray-100 text-gray-700 leading-tight"/>
		</label>
		<button on:click={handleClear} class="bg-blue-500 hover:bg-blue-700 text-black font-bold py-2 px-4 rounded">Clear</button>
	</div>

	<div id="tasks">
		<ul class="px-0 my-5 w-auto">
			{#each tasks as task,index (task.title)}
			<li style="position: relative;" class="border border-black list-none {task.done?'bg-blue-400':''} hover:bg-blue-100 rounded-md px-5 py-0">
				<label style="position: absolute; top: -999px; left: -999px;" for="task-{index}">Check task complete</label>
				<input id="task-{index}" bind:checked={task.done} type="checkbox" >
				<p class="font-serif text-lg {task.done ? 'line-through': ''}">{task.title}</p>
				<p class="text-red-500 hover:text-red-900" style="position: absolute; right: 1px;" on:click={removeTask(task.title)}>x</p>
			</li>
			{/each}
		</ul>

	</div>

	<div>
		<a href="/mypage">Next Page</a>
	</div>
</main>

<style>
	:root {
		font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell,
			'Open Sans', 'Helvetica Neue', sans-serif;
	}

	main {
		text-align: center;
		padding: 1em;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: lowercase;
		font-size: 4rem;
		font-weight: 100;
		line-height: 1.1;
		margin: 4rem auto;
		max-width: 14rem;
	}


	@media (min-width: 480px) {
		h1 {
			max-width: none;
		}

		p {
			max-width: none;
		}
	}
</style>

