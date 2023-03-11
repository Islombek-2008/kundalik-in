<template>
	<main>
		<h1>O'quvchilar davomati</h1>

		<form @submit.prevent="addWeight">
			<input
				type="number"
				id="input"
				v-model="weightInput"
				autocomplete="off"
				placeholder="100%"
			/>

			<button type="submit">Add</button>
		</form>

		<div v-if="weights && weights.length > 0">
			<h2>7 kunlik davomat</h2>

			<div class="canvas-box">
				<canvas ref="weightChartEl"></canvas>
			</div>

			<div class="weight-history">
				<h2>Davomat tarixi</h2>
				<ul>
					<li v-for="weight in weights">
						<p>{{ weight.weight }} <span>%</span></p>
						<small>
							{{ new Date(weight.date).toLocaleDateString() }}
						</small>
					</li>
				</ul>
			</div>
		</div>
	</main>
</template>

<script setup>
import { ref, shallowRef, computed, watch, nextTick } from "vue";
import Chart from "chart.js/auto";
const weights = ref([]);
const weightChartEl = ref(null);
const weightChart = shallowRef(null);
const weightInput = ref();
function addWeight() {
	if (weightInput.value !== "") {
		weights.value.unshift({
			weight: weightInput.value,
			date: new Date().getTime(),
		});
		weightInput.value = "";
	}
}
watch(
	weights,
	(newWeights) => {
		const ws = [...newWeights];
		if (weightChart.value) {
			weightChart.value.data.labels = ws
				.sort((a, b) => a.date - b.date)
				.map((weight) => new Date(weight.date).toLocaleDateString())
				.slice(-7);
			weightChart.value.data.datasets[0].data = ws
				.sort((a, b) => a.date - b.date)
				.map((weight) => weight.weight)
				.slice(-7);
			weightChart.value.update();
			return;
		}
		nextTick(() => {
			weightChart.value = new Chart(weightChartEl.value.getContext("2d"), {
				type: "line",
				data: {
					labels: ws
						.sort((a, b) => a.date - b.date)
						.map((weight) => new Date(weight.date).toLocaleDateString()),
					datasets: [
						{
							label: "Weight",
							data: ws
								.sort((a, b) => a.date - b.date)
								.map((weight) => weight.weight),
							backgroundColor: "hsla(220, 60%, 92%, 1)",
							borderColor: "#000",
							borderWidth: 1,
							fill: true,
						},
					],
				},
				options: {
					responsive: true,
					maintainAspectRatio: false,
				},
			});
		});
	},
	{ deep: true }
);
</script>

<style scoped>
main {
	width: 100%;
}
h1 {
	display: flex;
	justify-content: center;
}
main form {
	display: flex;
	justify-content: center;
	margin-top: 25px;
	position: relative;
}
main form input {
	width: 250px;
	height: 40px;
	font-size: 22px;
}
main form button {
	border-bottom-right-radius: 5px;
	border-top-right-radius: 5px;
	border: 0;
	height: 39px;
	font-size: 16px;
	margin-top: 0.4px;
}
.weight-history ul {
	margin-top: 20px;
}
.weight-history ul li {
	display: flex;
	width: 25%;
	justify-content: space-between;
	padding: 5px;
	font-size: 18px;
}
.weight-history ul li + li {
	border-top: 1px solid red;
}
@media (max-width: 476px) {
	main form {
		justify-content: start;
	}
	main form input {
		width: 150px;
		height: 30px;
		font-size: 16px;
	}
	main form button {
		border-bottom-right-radius: 5px;
		border-top-right-radius: 5px;
		border: 0;
		height: 30px;
		font-size: 14px;
		margin-top: 0.4px;
	}
	.weight-history ul li + li {
		border-top: 1px solid red;
	}
}
@media (max-width: 685px) {
	.weight-history ul li {
		display: flex;
		width: 100%;
		justify-content: space-between;
		padding: 5px;
		font-size: 16px;
	}
	.weight-history ul li + li {
		border-top: 1px solid red;
	}
}
</style>
