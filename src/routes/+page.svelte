<script>
	import ToDo from './todo.svelte';
</script>

<div class="main">
	<h1>Firebase Todo App</h1>
	<p>Do add your daily task and manage your day.</p>
	<ToDo />
</div>

<style>
	/* .main{
		box-sizing: border-box;
		box-shadow: 2%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		max-width: fit-content;
	} */
	.main{
		font-family: Georgia, 'Times New Roman', Times, serif;
		text-align: center;
	}
	h1{
		
		color: darkgreen;
	}
</style>