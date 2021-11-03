<script lang="ts" context="module">
	import type { Load } from '@sveltejs/kit';
	import type { Book } from 'src/types/book.interfaces';
	import BookTemplate from '../../components/BookTemplate.svelte';

	export const load: Load = async ({ page, fetch }) => {
		const { id } = page.params;
		const url = `https://www.googleapis.com/books/v1/volumes/${id}`;
		const res = await fetch(url);

		if (res.ok) {
			return {
				props: {
					book: await res.json(),
				},
			};
		}

		return {
			status: res.status,
			error: new Error(`Could not load ${url}`),
		};
	};
</script>

<script lang="ts">
	export let book: Book;
</script>

<BookTemplate {book} />

<style>
</style>
