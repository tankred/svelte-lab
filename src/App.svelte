<script>
  // import
  import Counter from "./Counter.svelte";
  import Updatearrays from "./Updatearrays.svelte";
  import Nested from './Nested.svelte'; 
  import Thing from './Thing.svelte';
  // export
  export let name;
  // let
  let user = { loggedIn: false };
  let x = 7;
  let cats = [
    { id: 'J---aiyznGQ', name: 'Keyboard Cat' },
    { id: 'z_AbfPXTKms', name: 'Maru' },
    { id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat' }
  ];
  let things = [
		{ id: 1, color: '#0d0887' },
		{ id: 2, color: '#6a00a8' },
		{ id: 3, color: '#b12a90' },
		{ id: 4, color: '#e16462' },
		{ id: 5, color: '#fca636' }
  ];

  function handleClick() {
    things = things.slice(1);
  }
  // login button
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
       <ul>
	{#each cats as cat, i}
	  <li><a target="_blank" href="https://www.youtube.com/watch?v={cat.id}">{i + 1}: {cat.name}</a></li>
	{/each}
       </ul>

<button on:click={handleClick}>
	Remove first thing
</button>

{#each things as thing}
	<Thing current={thing.color}/>
{/each}

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
