<script>
  import Counter from "./Counter.svelte";
  export let name;
  let x = 7;
  import Updatearrays from "./Updatearrays.svelte";
  import Nested from './Nested.svelte'; 
  // login button
  let user = { loggedIn: false };
  function toggle() {
    user.loggedIn = !user.loggedIn;
  }
</script>

<main>
	<h1>Hello {name}!</h1>
	<Counter />
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
	<Updatearrays />
	<Nested />
	<Nested answer={41} />
	{#if user.loggedIn}
	<button on:click={toggle}>
	  Log out
	</button>
        {/if}

       {#if !user.loggedIn}
	<button on:click={toggle}>
	  Log in
	</button>
       {/if}	
	<!-- shorthand: using else -->
       {#if user.loggedIn}
	<button on:click={toggle}>
	  Log out
	</button>
       {:else}
	<button on:click={toggle}>
	  Log in
	</button>
       {/if}
       {#if x > 10}
	<p>{x} is greater than 10</p>
       {:else if 5 > x}
	<p>{x} is less than 5</p>
       {:else}
	<p>{x} is between 5 and 10</p>
       {/if}
</main>

<style>
  body {
    background-color: #0f0;
  }
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
