<template>
	<div class="d-flex w-100 flex-wrap">
		<template v-for="vacancy in currentVacancies" :key="vacancy.vacancy_id">
			<div class="card col-3 m-2">
				<div class="location">{{ vacancy.placetitle }}</div>
				<h4 class="proftitle">{{ vacancy.proftitle }}</h4>
				<p>{{ vacancy.salary_volume + ' ' + vacancy.salary_type_title }}</p>
				<p>{{ vacancy.directiontitle }}</p>
				<p>{{ vacancy.clientname }}</p>
				<div class="buttons">
					<button class="button" @click="selectVacancy">Откликнуться!</button>
				</div>
			</div>
		</template>
	</div>
</template>

<script setup>
import { defineProps, computed } from "vue";


const props = defineProps({
	vacancies: Array,
})

const emit = defineEmits(['select'])

const currentVacancies = computed(() => props.vacancies)

function selectVacancy() {
    emit('select')
}


</script>

<style scoped>
.card {
	background-color: #fff;
	border-radius: 10px;
	box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
	padding: 20px;
	max-width: 300px;
	width: 100%;
}

.card p {
	margin-bottom: 20px;
	font-size: 14px;
	color: #000;
	font-weight: bold;
}

.location {
	color: #9e9e9e;
	font-size: 14px;
	margin-bottom: 20px;
	font-weight: bold;
}

.proftitle {
	margin-bottom: 10px;
	padding-bottom: 10px;
	border-bottom: 1px solid #000;
}


.button {
	border: none;
	padding: 5px;
	width: 100%;
	background: #eb8000;
	text-align: center;
	color: #fff;
	border-radius: 6px;
}
</style>