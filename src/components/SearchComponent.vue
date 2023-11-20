<template>
	<div class="search-bar">
		<input
			type="text"
			v-model="searchQuery"
			@input="emitSearch"
			class="form-control"
			placeholder="Search for movies..."
		/>
	</div>
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
.search-bar {
	margin-bottom: 20px;
}

/* Further customization if needed */
</style>
