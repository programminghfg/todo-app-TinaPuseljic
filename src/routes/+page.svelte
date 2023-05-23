<script>
	import { onMount, afterUpdate } from 'svelte';
	let newItem = '';
	let todos = [];

	onMount(() => {
		const storedTodos = localStorage.getItem('todos');
		if (storedTodos) {
			todos = JSON.parse(storedTodos);
		}
	});

	afterUpdate(() => {
		localStorage.setItem('todos', JSON.stringify(todos));
	});
    
	function addToList() {
		todos = [...todos, { text: newItem, status: false }];

		newItem = '';
	}

	function removeFromList(index) {
		todos.splice(index, 1);
		todos = todos;
	}

	function removeAllTodos() {
		todos = [];
	}

	function handleKeydown(event) {
		if (event.key === 'Enter') {
			addToList();
		}
	}
</script>

<input bind:value={newItem} type="text" placeholder="new todo item.." on:keydown={handleKeydown} />
<button on:click={addToList}>Add</button>
<button on:click={removeAllTodos}>Remove All</button>

<br />
{#each todos as item, index}
	<input bind:checked={item.status} type="checkbox" />
	<span class:checked={item.status}>{item.text}</span>
	<span on:click={() => removeFromList(index)}>❌</span>
	<br />
{/each}

<style>
	.checked {
		text-decoration: line-through;
	}
</style>
