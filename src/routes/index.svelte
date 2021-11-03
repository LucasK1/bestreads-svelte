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

<svelte:head>
	<title>Bestreads</title>
</svelte:head>

<div class="wrapper">
<header>
	<h1>Bestreads</h1>
	<h2>This will be your best alternative to THE OTHER site</h2>
</header>
<main>
	<form on:submit|preventDefault={onSubmit}>
		<input type="text" placeholder="Search for books..." bind:value={input} />
	</form>
	{#if promise}
		{#await promise}
			<div class="spinner-container">Loading...</div>
		{:then books}
			<div class="searchedBooks-container">
				<SearchedBooks {books} />
			</div>
		{:catch err}
			<p>{err}</p>
		{/await}
	{/if}
</main>
</div>

<style>
	.wrapper {
		background-color: #f8f5f2;
		min-height: 100vh;
	}

	header {
		color: #232323;
		text-align: left;
	}

	h1 {
		margin:  0;
		font-size: 3rem;
	}

	h2 {
		font-size: 2rem;
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
