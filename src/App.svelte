<script>
  // import
  import Counter from "./Counter.svelte";
  import Updatearrays from "./Updatearrays.svelte";
  import Nested from './Nested.svelte'; 
  import Thing from './Thing.svelte';
  //import Inner from './Inner.svelte';
  import Outer from './Outer.svelte';
  import Custombutton from './Custombutton.svelte';
  // export
  export let name;
  // let
  let questions = [
		{ id: 1, text: `Where did you go to school?` },
		{ id: 2, text: `What is your mother's name?` },
		{ id: 3, text: `What is another personal fact that an attacker could easily find with Google?` }
  ];
  let selected;
  let answer = '';
  let value = `Some words are *italic*, some are **bold**`;
  let scoops = 1;
  let flavours = ['Mint choc chip'];
  let menu = [
	'Cookies and cream',
	'Mint choc chip',
	'Raspberry ripple'
  ];
  let yes = false;
  let a = 1;
  let b = 2;
  let namebinding = 'world';
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
  function handleSubmit() {
		alert(`answered question ${selected.id} (${selected.text}) with "${answer}"`);
  }
  function join(flavours) {
    if (flavours.length === 1) return flavours[0];
    return `${flavours.slice(0, -1).join(', ')} and ${flavours[flavours.length - 1]}`;
  }
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

  function handleClickCB() {
		alert('clicked');
  }

  function toggle() { // login button
    user.loggedIn = !user.loggedIn;
  }
</script>

<main>
  <div id="container">
	<h1>Hello {name}!</h1>
	<h2>Insecurity questions</h2>

    <form on:submit|preventDefault={handleSubmit}>
    	<select bind:value={selected} on:change="{() => answer = ''}">
    		{#each questions as question}
    			<option value={question}>
    				{question.text}
    			</option>
    		{/each}
    	</select>
    
    	<input bind:value={answer}>
    
    	<button disabled={!answer} type=submit>
    		Submit
    	</button>
    </form>
    
    <p>selected question {selected ? selected.id : '[waiting...]'}</p>
    
	<textarea bind:value></textarea>
	<h2>Size</h2>
	<label>
	  <input type=radio bind:group={scoops} value={1}>
  	  One scoop
	</label>
	<label>
	<input type=radio bind:group={scoops} value={2}>
	Two scoops
	</label>
	<label>
	<input type=radio bind:group={scoops} value={3}>
	Three scoops
	</label>
	<h2>Flavours</h2>
	<select multiple bind:value={flavours}>
	{#each menu as flavour}
		<option value={flavour}>
			{flavour}
		</option>
	{/each}
        </select>
	{#each menu as flavour}
	<label>
		<input type=checkbox bind:group={flavours} value={flavour}>
		{flavour}
	</label>
	{/each}
	{#if flavours.length === 0}
	<p>Please select at least one flavour</p>
{:else if flavours.length > scoops}
	<p>Can't order more flavours than scoops!</p>
{:else}
	<p>
		You ordered {scoops} {scoops === 1 ? 'scoop' : 'scoops'}
		of {join(flavours)}
	</p>
{/if}
	<label>
	<input type=checkbox bind:checked={yes}>
	Yes! Send me regular email spam
	</label>
	{#if yes}
	<p>Thank you. We will bombard your inbox and sell your personal details.</p>
	{:else}
	<p>You must opt in to continue. If you're not paying, you're the product.</p>
	{/if}
	<button disabled={!yes}>
	Subscribe
	</button>
	<label>
	<input type=number bind:value={a} min=0 max=10>
	<input type=range bind:value={a} min=0 max=10>
	</label>
	<label>
	<input type=number bind:value={b} min=0 max=10>
	<input type=range bind:value={b} min=0 max=10>
	</label>
	<p>{a} + {b} = {a + b}</p>
	<input value={namebinding}>
	<h2>Hello {namebinding}!</h2>
	<Custombutton on:click={handleClickCB}/>
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

  textarea { width: 100%; height: 200px; }
  input { display: block; width: 500px; max-width: 100%; }
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
