<script lang="ts">
	import type { Book } from 'src/types/book.interfaces';

	export let book: Book;
</script>

<main>
	<img src={book.volumeInfo.imageLinks.thumbnail} alt="" />
</main>

<style lang="scss">
</style>
