<template>
	<div class="todo">
		<h2>O'quvchi I.F</h2>
		<ul>
			<li v-for="todo in todos" :key="todo.id">
				<i class="bi bi-person-fill"></i>
				{{ todo.title }}
				<button @click="deleteTodo(todo)">
					<i class="bi bi-person-fill-dash"></i>
				</button>
			</li>
		</ul>
		<form @submit.prevent="addTodo">
			<input type="text" v-model="newTodo" placeholder="ism familya" />
			<button type="submit">Add <i class="bi bi-person-fill-add"></i></button>
		</form>
	</div>
</template>

<script>
import axios from "axios";

export default {
	data() {
		return {
			todos: [],
			newTodo: "",
		};
	},
	created() {
		this.fetchTodos();
	},
	methods: {
		fetchTodos() {
			const todos = JSON.parse(localStorage.getItem("todos")) || [];
			this.todos = todos;
		},
		saveTodos() {
			localStorage.setItem("todos", JSON.stringify(this.todos));
		},
		addTodo() {
			if (this.newTodo.trim()) {
				const newTodo = {
					id: Date.now(),
					title: this.newTodo,
					completed: false,
				};
				this.todos.push(newTodo);
				this.saveTodos();
				this.newTodo = "";
			}
		},
		updateTodo(todo) {
			axios
				.put(`https://jsonplaceholder.typicode.com/todos/${todo.id}`, todo)
				.then(() => {
					this.saveTodos();
				})
				.catch((error) => {
					console.error(error);
				});
		},
		deleteTodo(todo) {
			axios
				.delete(`https://jsonplaceholder.typicode.com/todos/${todo.id}`)
				.then(() => {
					this.todos.splice(this.todos.indexOf(todo), 1);
					this.saveTodos();
				})
				.catch((error) => {
					console.error(error);
				});
		},
	},
};
</script>
<style scoped>
.todo {
	width: 100%;
}
.todo h2 {
	display: flex;
	justify-content: center;
	margin-bottom: 25px;
}
.todo ul {
	columns: 3;
}
.todo ul li {
	list-style-type: none;
	font-size: 20px;
	width: 370px;
	border: 1px solid red;
	margin-bottom: 10px;
	padding: 7px 5px;
	border-radius: 5px;
	display: flex;
	justify-content: space-between;
}
.todo ul li button {
	border: 0;
	font-size: 18px;
	background: transparent;
	color: red;
	cursor: pointer;
}

.todo form {
	display: flex;
	justify-content: center;
	margin-top: 25px;
	position: relative;
}
.todo form input {
	width: 300px;
	height: 40px;
	font-size: 22px;
}
.todo form button {
	border-bottom-right-radius: 5px;
	border-top-right-radius: 5px;
	border: 0;
	height: 39px;
	font-size: 16px;
	margin-top: 0.4px;
}
@media (max-width: 1262px) {
	.todo {
		width: 100%;
		align-items: flex-start;
	}

	.todo ul {
		display: flex;
		flex-direction: column;
		width: 100%;
		align-items: flex-start;
		padding: 0;
	}
}
@media (max-width: 476px) {
	.todo ul li {
		list-style-type: none;
		font-size: 18px;
		width: 270px;
		border: 1px solid red;
		margin-bottom: 10px;
		padding: 5px 3px;
		border-radius: 5px;
		display: flex;
		justify-content: space-between;
	}

	.todo form {
		justify-content: start;
	}
	.todo form input {
		width: 150px;
		height: 30px;
		font-size: 16px;
	}
	.todo form button {
		border-bottom-right-radius: 5px;
		border-top-right-radius: 5px;
		border: 0;
		height: 30px;
		font-size: 14px;
		margin-top: 0.4px;
	}
}
</style>
