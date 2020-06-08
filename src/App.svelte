<script>
  import Counter from "./Counter.svelte";
  export let name;
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
