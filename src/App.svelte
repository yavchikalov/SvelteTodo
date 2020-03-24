<Header />
<main class="main">
	<div class="todo">
		<form on:submit|preventDefault={handleSubmit} class="todo__form">
			<input
				bind:value={todoText}
				placeholder="Введите название"
				class="todo__input"
			>
		</form>
		<div class="todo__list">
			{#each filtered as todo (todo.id)}
			<label class="todo__item {todo.done ? 'todo__item--done' : ''}">
				<input
					type="checkbox"
					bind:checked={todo.done}
					class="todo__checkbox"
				>
				<span class="todo__checkmark"></span>
				{ todo.text }
			</label>
			{/each}
		</div>
		<div class="todo__filter">
			<div class="todo__item-filter {currentFilter === 'all' ? 'todo__item-filter--active' : ''}" on:click={() => handleFilter('all')}>Все</div>
			<div class="todo__item-filter {currentFilter === 'active' ? 'todo__item-filter--active' : ''}" on:click={() => handleFilter('active')}>Активные</div>
			<div class="todo__item-filter {currentFilter === 'done' ? 'todo__item-filter--active' : ''}" on:click={() => handleFilter('done')}>Завершенные</div>
		</div>
	</div>
</main>

<script>
	import Header from './templates/Header.svelte';
	let todos = [
		{
			id: getId(),
			text: 'Прочтитать документацию svelte',
			done: true
		},
		{
			id: getId(),
			text: 'Посмотреть презентации про svelte',
			done: true
		},
		{
			id: getId(),
			text: 'Попробовать реализовать самый простой todo на svelte',
			done: true
		},
		{
			id: getId(),
			text: 'Ждать',
			done: false
		}
	];

	let todoText = '';

	let currentFilter = 'all';

	$: filtered = currentFilter === 'all'
		? todos
		: currentFilter === 'done'
			? todos.filter(item => item.done)
			: todos.filter(item => !item.done);
	
	function getId() {
		return Math.random().toString(16).substring(2);
	}

	function handleSubmit() {
		if (todoText) {
			todos = [...todos, {
				id: getId(),
				text: todoText,
				done: false
			}];
			todoText = '';
		}
	}

	function handleFilter(filter) {
		currentFilter = filter;
	}
</script>

<style>
.main {
	margin-bottom: 100px;
}

.todo {
	max-width: 600px;
    margin: 0 auto;
}

.todo__list {
	padding: 24px 16px;
	box-shadow: 0px 0px 10px 10px silver;
	border-radius: 8px;
}

.todo__form {
	width: 100%;
    max-width: 600px;
    margin-bottom: 40px;
}

.todo__input {
    font-size: 1.4rem;
    border-radius: 8px;
    box-shadow: inset 0px 0px 5px 0px silver;
    width: 100%;
}

.todo__item {
	font-size: 1.4rem;
	display: flex;
	position: relative;
	margin-bottom: 12px;
	cursor: pointer;
	margin-bottom: 16px;
	transition: opacity .3s;
}

.todo__item:last-child {
	margin-bottom: 0;
}

.todo__item--done {
	opacity: .5;
}

.todo__item:hover {
	background: rgba(198, 194, 194, 0.52);
    border-radius: 8px;
}

.todo__checkbox {
	position: absolute;
	opacity: 0;
	cursor: pointer;
	height: 0;
	width: 0;
}

.todo__checkmark {
    border: 2px solid rgb(180, 159, 159);
    width: 30px;
    height: 30px;
    display: block;
    position: relative;
	border-radius: 8px;
	margin-right: 8px;
	flex-shrink: 0;
}

.todo__item:hover .todo__checkmark:after,
.todo__checkbox:checked ~ .todo__checkmark:after {
	content: '';
	display: block;
    left: 10px;
    top: 6px;
    width: 7px;
    height: 12px;
    border: solid green;
    border-width: 0 3px 3px 0;
	transform: rotate(45deg);
	position: absolute;
}

.todo__filter {
    margin-top: 24px;
	display: flex;
	font-size: 1rem;
}

.todo__item-filter {
	opacity: .7;
	transition: opacity .3s;
	cursor: pointer;
	font-style: italic;
	margin: 0 16px;
}

.todo__item-filter--active {
	font-weight: bold;
	opacity: 1;
}

.todo__item-filter:hover {
	opacity: 1;
}

</style>
