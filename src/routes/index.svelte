<script lang="ts">
	import axios from 'axios';
	import type { Book } from 'src/types/book.interfaces';
	import SearchedBooks from '../components/SearchedBooks.svelte';

	let input = '';
	async function fetchBooks(input: string) {
		try {
			const { data } = await axios.get<{ items: Book[] }>(
				`https://www.googleapis.com/books/v1/volumes?q=${input
					.split(' ')
					.join('+')}&maxResults=30&langRestrict=en`,
			);
			return data.items;
		} catch (err) {
			throw new Error(err);
		}
	}
	let promise: Promise<Book[]> = null;
	function onSubmit() {
		promise = fetchBooks(input);
	}
</script>

<main>
	<header>
		<h1>Welcome to Bestreads!</h1>
		<h2>This will be your best alternative to THE OTHER site</h2>
	</header>
	<form on:submit|preventDefault={onSubmit}>
		<input type="text" placeholder="Search for books..." bind:value={input} />
	</form>
	{#if promise}
		{#await promise}
			<div class="spinner-container">Loading...</div>
		{:then books}
			{#each books as book}
				<div class="searchedBooks-container">
					<SearchedBooks {books} />
				</div>
			{/each}
		{:catch err}
			<p>{err}</p>
		{/await}
	{/if}
</main>

<style>
	main {
		padding-top: 8vh;
		height: 92vh;
		width: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		background: url('/1.jpg') no-repeat center;
		background-size: cover;
	}

	header {
		color: #fff;
		height: 20vh;
		text-align: center;
	}

	h1 {
		font-size: 4rem;
		line-height: 1rem;
	}

	h2 {
		font-size: 2rem;
		line-height: 1rem;
	}

	input {
		margin: 30px 0;
		width: 600px;
		height: 60px;
	}
	.spinner-container {
		height: 80vh;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.searchedBooks-container {
		height: 80vh;
	}
</style>
