<script>
	import { count } from './stores.js';

	function decrement() { // decrement the count
	  count.update(n => n - 1);
	}
</script>

<button on:click={decrement}>
	-
</button>
