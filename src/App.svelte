<script>
  // import
  import Counter from "./Counter.svelte";
  import Updatearrays from "./Updatearrays.svelte";
  import Nested from './Nested.svelte'; 
  import Thing from './Thing.svelte';
  //import Inner from './Inner.svelte';
  import Outer from './Outer.svelte';
  // export
  export let name;
  // let
  let m = { x: 0, y: 0 };
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
  let promise = getRandomNumber();

  // function
  function handleMessage(event) {
		alert(event.detail.text);
  }
  function handleClickOnce() {
		alert('clicked')
  }

  function handleMousemove(event) {
		m.x = event.clientX;
		m.y = event.clientY;
  }

  async function getRandomNumber() {
    const res = await fetch(`https://svelte.dev/tutorial/random-number`);
    const text = await res.text();

		if (res.ok) {
			return text;
		} else {
			throw new Error(text);
		}
   }

   function handleClickPromise() {
     promise = getRandomNumber();
   }

  function handleClick() {
    things = things.slice(1);
  }
  // login button
  function toggle() {
    user.loggedIn = !user.loggedIn;
  }
</script>

<main>
  <div id="container">
	<h1>Hello {name}!</h1>
	<Outer on:message={handleMessage}/>
	<button on:click|once={handleClickOnce}>
	Click me
	</button>
	<div on:mousemove="{e => m = { x: e.clientX, y: e.clientY }}">
 	The mouse position is {m.x} x {m.y}
        </div>
	<div on:mousemove={handleMousemove}>
        	The mouse position is {m.x} x {m.y}
        </div>
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

<button on:click={handleClickPromise}>
	generate random number
</button>

{#await promise then value}
	<p>the value is {value}</p>
{/await}

{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}

  </div>
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
  #container {
    margin: 1.24em;
    display: grid;
    grid-gap: 1.53rem;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  }


  /*
  Auto-fit vs auto-fill: 
  https://codepen.io/tulsi-prasad/pen/XWmddjK
   */

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
