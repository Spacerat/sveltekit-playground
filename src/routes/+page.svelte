
<script>
  import Todo from './Todo.svelte';

	import { fade, slide, crossfade } from 'svelte/transition';
	import { flip } from 'svelte/animate';

	let lastId = 0;

	/** @type {{name: string, completed: boolean, id: number}[]} */
	let todos = [
		{ name: 'A todo item', completed: false, id: -1 },
		{ name: "Animations are fun", completed: false, id: -2 },
		{ name: 'Whee', completed: false, id: -3 }
	];

	let newTodo = '';

	function addTodo() {
		lastId += 1
		todos = [...todos, { name: newTodo, completed: false, id: lastId }];
		newTodo = '';
	}

	$: outstanding = todos.filter(x => !x.completed)
	$: completed = todos.filter(x => !!x.completed)

	const [send, receive] = crossfade({
		fallback: (node) => fade(node)
	})
</script>

<h1>Svelte todos and animations and stuff</h1>


<form on:submit|preventDefault={addTodo} >
	<input bind:value={newTodo} />
	<button type="submit">Add</button>
</form>


<div class="columns">
	<div>
		<h2>Todos</h2>
		<ul>
			{#each outstanding as todo (todo.id)}
				<li class="contents" animate:flip in:send={{key: todo.id}} out:receive={{key: todo.id}}>
					<Todo bind:todo={todo}></Todo>
				</li>
			{/each}
		</ul>
	</div>
	<div>
		<h2>Completed</h2>
		<ul>
			{#each completed as todo (todo.id)}
				<li class="contents" animate:flip in:send={{key: todo.id}} out:receive={{key: todo.id}}>
					<Todo bind:todo={todo}></Todo>
				</li>
			{/each}
		</ul>
	</div>
</div>

<style>
	ul {
		padding: 0;
	}

	li {
		background-color: white;
		margin-bottom: 1em;
		border: thin solid gray;
		display: flex;
		box-shadow: 2px 2px 5px;
		flex-direction: row;
		align-items: center;
		list-style-type: none;
		padding: 0.5em;
		border-radius: 5px;
	}

	.columns {
		display: flex;
		flex-direction: row;
		gap: 1em;
	}
</style>
