<template>
	<form class="text-center my-5" @submit.prevent="emitSearch" name="searchView">
		<div class="form-group my-3">
			<label for="inputTitle">Title</label>
			<input type="text" v-model="titleQuery" id="inputTitle" class="form-control"
				placeholder="Input part or full title of the movie" />
		</div>
		<div class="form-group row my-3">
			<label class="col-sm-2 col-form-label" for="inputProductionFrom">Production Year from:</label>
			<div class="col-sm-10">
				<input type="number" v-model.number="yearFrom" id="inputProductionFrom" class="form-control"
					placeholder="Natural number between 2000 and 2023" />
			</div>
		</div>
		<div class="form-group row my-3">
			<label class="col-sm-2 col-form-label" for="inputProductionTo">Production Year to:</label>
			<div class="col-sm-10">
				<input type="number" v-model.number="yearTo" id="inputProductionTo" class="form-control"
					placeholder="Natural number between 2000 and 2023" />
			</div>
		</div>
		<div class="form-group my-3">
			<label for="inputCast">Cast</label>
			<input type="text" v-model="castQuery" id="inputCast" class="form-control" placeholder="First and last name" />
		</div>
		<div class="form-group row mx-0">
			<input type="submit" class="btn btn-info col-sm-12" value="Search" />
			<input type="button" class="btn btn-secondary col-sm-12 mt-1" value="Clear" @click="clearSearch" />
		</div>
	</form>
</template>

<script>
import { ref, defineComponent } from 'vue';

export default defineComponent({
	setup(props, { emit }) {
		const titleQuery = ref('');
		const yearFrom = ref(null);
		const yearTo = ref(null);
		const castQuery = ref('');

		const emitSearch = () => {
			emit('search', {
				title: titleQuery.value,
				yearFrom: yearFrom.value,
				yearTo: yearTo.value,
				cast: castQuery.value,
			});
		};

		const clearSearch = () => {
			titleQuery.value = '';
			yearFrom.value = null;
			yearTo.value = null;
			castQuery.value = '';
			emit('clear');
		};

		return {
			titleQuery,
			yearFrom,
			yearTo,
			castQuery,
			emitSearch,
			clearSearch,
		};
	},
});
</script>

<style scoped>
/* Further customization if needed */
</style>
