<template>
	<form class="my-5" @submit.prevent="emitSearch" name="searchView">
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
		<div class="form-group input-center row my-3">
			<label for="inputCast">Cast:</label>
			<label class="col-sm-1"><input type="radio" v-model="castSearchType" value="all" /> All Actors</label>
			<label class="col-sm-1"><input type="radio" v-model="castSearchType" value="any" /> Any Actor</label>
			<div class="col-sm-10">
				<input type="text" v-model="castQuery" id="inputCast" class="form-control"
					placeholder="First and last name" />
			</div>
		</div>
		<div class="form-group row mx-0">
			<input type="submit" class="btn btn-info col-sm-12" value="Search" />
			<input type="button" class="btn btn-secondary col-sm-12 mt-1" value="Clear" @click="clearSearch" />
		</div>
	</form>
</template>

<script>
export default {
	name: 'SearchComponent',
	data() {
		return {
			titleQuery: '',
			yearFrom: null,
			yearTo: null,
			castQuery: '',
			castSearchType: 'any'
		};
	},
	methods: {
		emitSearch() {
			this.$emit('search', {
				title: this.titleQuery,
				yearFrom: this.yearFrom,
				yearTo: this.yearTo,
				cast: this.castQuery,
				castSearchType: this.castSearchType
			});
		},
		clearSearch() {
			this.titleQuery = '';
			this.yearFrom = null;
			this.yearTo = null;
			this.castQuery = '';
			this.castSearchType = 'any';
			this.$emit('clear', {
				title: this.titleQuery,
				yearFrom: this.yearFrom,
				yearTo: this.yearTo,
				cast: this.castQuery,
				castSearchType: this.castSearchType
			});
		}
	}
};
</script>

<style scoped>
.input-center {
	position: relative;
	display: flex;
	align-items: center;
}
</style>
