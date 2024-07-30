<template>
	<div class="filter">
		<div>
			<h4>{{ props.filterName }}</h4>
			<select @change="selectItem">
				<option v-for="(option, index) in currentFilterOptions" :key="index">
					{{ option }}
				</option>
			</select>
		</div>
	</div>
</template>

<script setup>
import { ref, defineProps, computed } from "vue";

const selectedItem = ref(null);

const props = defineProps({
	filterOptions: Array,
	filterName: String,
})

const emit = defineEmits(['select-option'])

const currentFilterOptions = computed(() => props.filterOptions)

const selectItem = () => {
	// selectedItem.value = event.target.value;
    emit('select-option')
};


</script>

<style scoped>
.filter select {
	width: 100%;
	padding: 10px;
	margin: 5px 0 22px 0;
	display: inline-block;
	border: none;
	background: #f1f1f1;
	font-size: 16px;
	outline: none;
	border-radius: 10px;
}
</style>