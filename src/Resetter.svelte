<script>
	import { count } from './stores.js';

	function reset() { // reset the count
	  count.set(0);
	}
</script>

<button on:click={reset}>
	reset
</button>
