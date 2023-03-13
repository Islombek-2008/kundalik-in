<template>
	<div class="container" ref="container">
		<div class="dark-light">
			<button @click="changeTitle">
				<i class="bi bi-brightness-high-fill"></i>
			</button>
			<button @click="changeTitle2"><i class="bi bi-moon-fill"></i></button>
		</div>
		<div>
			<h1 ref="pageTitle">{{ title }}</h1>
		</div>
		<hr />
		<Todo></Todo>
		<hr />
		<ChartComponent />
	</div>
</template>

<script>
import Todo from "./components/todo.vue";
import ChartComponent from "./components/chart.vue";
export default {
	data() {
		return {
			title: "Kundalik",
		};
	},
	components: {
		Todo,
		ChartComponent,
	},
	methods: {
		changeTitle() {
			this.$refs.container.style.background = "white";
			this.$refs.container.style.color = "black";
			this.changeTitle.style.color = "black";
		},
		changeTitle2() {
			this.$refs.container.style.background = "#3e3d32";
			this.$refs.container.style.color = "white";
			this.changeTitle2.style.color = "white";
		},
	},
};
</script>
<style>
.dark-light {
	position: absolute;
	right: 20px;
	top: 10px;
}
.dark-light button {
	font-size: 25px;
	border: 0;
	background: transparent;
	cursor: pointer;
}
.container {
	padding: 50px;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}
hr {
	width: 100%;
	margin: 50px 10px;
}
@media (max-width: 1262px) {
	.container {
		padding: 50px 10px;
	}
}
</style>
