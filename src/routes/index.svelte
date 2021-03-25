<script>
	// import Counter from '$lib/Counter.svelte';
	import TailwindCss from '$lib/TailwindCSS.svelte';

	let tasks = [];
	if (typeof window !== 'undefined'){
		let tasksFromStorage = localStorage.getItem('tasks');
		tasks = tasksFromStorage ? JSON.parse(tasksFromStorage): []
	}
	
	$: {
		if (typeof window !== 'undefined') {
			document.title = 'ToDo App'
			window.localStorage.setItem('tasks', JSON.stringify(tasks))
		}
	}

	let task='';

	function handleNewTask() {
		tasks = [...tasks, {title:task, done: false, edit: false}]
		task = '';
	}

</script>
<TailwindCss />

<main>
	<h1>ToDo!</h1>

	<div class="mb-3 pt-0 w-2/4 m-auto">
		<input type="text" on:change={handleNewTask} bind:value={task} placeholder="Enter new task" class="px-3 py-3 placeholder-gray-400 text-gray-700 relative bg-white bg-white rounded text-sm shadow outline-none focus:outline-none focus:shadow-outline w-full"/>
	</div>

	<div id="tasks">
		<ul>
			{#each tasks as task}
			<li>
				<input bind:checked={task.done} type="checkbox" >
				<p class="font-serif text-lg {task.done ? 'line-through': ''}">{task.title}</p>
			</li>
			{/each}
		</ul>

	</div>
	<!-- <Counter />

	<p>Visit <a href="/mypage">svelte.dev</a> to learn how to build Svelte apps.</p> -->
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

	p{
		display: inline-block;
	}

	#tasks {
		max-width: 50%;
		display: flex;
		justify-content: center;
		margin: 0 auto;
	}

	#tasks ul li{
		display: flex;
		align-items: center;
		border-bottom: 2px solid black;
		margin-bottom: 10px;
	}

	#tasks ul li>*{
		padding: 10px;
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

