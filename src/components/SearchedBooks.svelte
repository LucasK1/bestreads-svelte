<script lang="ts">
	import type { Book } from 'src/types/book.interfaces';

	export let books: Book[];
</script>

<section>
	{#if books.length > 0}
		{#each books as { id, volumeInfo } (id)}
			<!-- Link -->
			{#if !!volumeInfo.imageLinks}
				<img class="book-cover" src={volumeInfo.imageLinks.smallThumbnail} alt="" />
			{:else}
				<div class="no-cover">
					<span class="title">{volumeInfo.title}</span>
					{#if !!volumeInfo.authors}
						<span class="authors">by {volumeInfo.authors[0]}</span>
					{/if}
				</div>
			{/if}
		{/each}
	{:else}
		<p>Nothing found</p>
	{/if}
</section>

<style lang="scss">
	section {
		/* height: 100%; */
		max-width: 1200px;
		display: grid;
		grid-template-columns: repeat(8, 1fr);
		gap: 20px;
		transition: height 1s linear;
	}

	.book-cover,
	.no-cover {
		height: 200px;
		width: 128px;
		border-radius: 3px;
		box-shadow: 2px 2px 20px #ccc;
		position: relative;
		z-index: 280;

		&:hover {
			transform: scale(1.2);
			transition: transform 300ms;
			transition-delay: 30ms;
			cursor: pointer;
		}
	}


	.no-cover {
		display: inline-block;
		color: #000;
		font-size: 0.7rem;
		text-align: center;
		border: 1px solid #ccc;
		background-color: #fff;
	}
</style>
