<script>
	import { initializeApp } from 'firebase/app';
	import { getFirestore, collection, onSnapshot, doc, updateDoc, deleteDoc, addDoc } from 'firebase/firestore';
	import { firebaseConfig } from '$lib/firebaseConfig';

	const firebaseApp = initializeApp(firebaseConfig);
	const db = getFirestore();

	// console.log(firebaseApp, db);
	const colRef = collection(db, 'todos');
    let todos = [];

	const unsubscribe = onSnapshot(colRef, (querySnapshot) => {
		let firebaseTodos = [];
        querySnapshot.forEach((doc) => {
            let todo = {...doc.data(), id:doc.id};
			firebaseTodos = [todo, ...firebaseTodos];
		});
		todos = firebaseTodos;
	});

	// let todos = [];
	let text = '';
	let error = '';

	const addTodo = async () => {
		if (text !== '') {
			const docRef = await addDoc(collection(db, "todos"), {
                text: text,
			    status: false
            });
			error = '';
		} else {
			error = 'Task is empty';
		}
	};

	const markTodoAsComplete = async (item) => {
		await updateDoc(doc(db, "todos", item.id), {
            status: !item.status
        });
	};

	const deleteTodo = async (id) => {
		await deleteDoc(doc(db, "todos", id));
	};

	const keyIsPressed = (event) => {
		if (event.key === 'Enter') {
			addTodo();
		}
	};
	$: console.table(todos);
</script>

<div class="container">
    <div class="block1">
        <input class="input" type="text" placeholder="Add a task" bind:value={text} />
        <button class="btn" on:click={addTodo}>Add</button>
    </div>
<ol class="block2">
	{#each todos as item}
		<li class:complete={item.status}>
			<span>
				{item.text}
			</span>
			<span>
				<button on:click={() => markTodoAsComplete(item)}>✅Done</button>
				<!-- <button on:click={() => deleteTodo(item.id)}>✘</button> -->
				<button on:click={() => deleteTodo(item.id)}>❎Delete</button>

			</span>
		</li>
	{:else}
		<p>No todos for set now.</p>
	{/each}
	<p class="error">{error}</p>
</ol>
</div>
<svelte:window on:keydown={keyIsPressed} />

<style>
    .container {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        /* margin: 40% 0 0 40%; */
		margin: 6% 0 6% 0;
    }
    .block1 {
        display: flex;
        justify-content: space-between;
    }
	.input {
		background-color: whitesmoke;
		color: brown;
		box-sizing: border-box;
		padding: 10px 20px;
		font-size: large;
		font-style: normal;
		font-weight: 600;
	}
	.btn {
		padding-left: 4px;
		margin-left: 4px;
	}

	.complete {
		text-decoration: line-through;
	}
	.error {
		color: red;
	}
	.block2{
		display: flex;
		flex-direction: column;
		max-width: 60%;
		font-weight: 500;
		font-size: medium;
		color: brown;
		margin: 16px 6px 16px 0;
		padding: 8px 8px 8px 0;

	}
	.block2 span button{
		margin: 8px;
	}
</style>
