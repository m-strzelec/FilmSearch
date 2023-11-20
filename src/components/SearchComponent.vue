<template>
	<form class="text-center my-5">
		<div class="form-group my-3">
			<label for=inputTitle>Title</label>
			<input type="text" v-model="searchQuery" @input="emitSearch" id=inputTitle class="form-control"
				placeholder="Input part or full title of the movie" />
		</div>
		<div class="form-group row my-3">
			<label class="col-sm-2 col-form-label" for="inputProductionFrom">Production Year from:</label>
			<div class="col-sm-10">
				<input type="text" id=inputProductionFrom class="form-control"
					placeholder="Natural number between 1900 and 2019" />
			</div>
		</div>
		<div class="form-group row my-3">
			<label class="col-sm-2 col-form-label" for="inputProductionTo">Production Year to:</label>
			<div class="col-sm-10">
				<input type="text" id=inputProductionTo class="form-control"
					placeholder="Natural number between 1900 and 2019" />
			</div>
		</div>
		<div class="form-group my-3">
			<label for="inputCast">Cast</label>
			<input type="text" id="inputCast" class="form-control" placeholder="First and last name" />
		</div>
		<div class="form-group row mx-0">
			<input type="button" class="btn btn-info col-sm-12" value="Search" />
		</div>
	</form>
</template>

<script>
export default {
	name: 'SearchComponent',
	data() {
		return {
			searchQuery: '',
		};
	},
	methods: {
		emitSearch() {
			this.$emit('search', this.searchQuery);
		},
	},
};
</script>

<style scoped>

/* Further customization if needed */</style>
