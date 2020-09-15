<script>
  // import
  import { onMount } from 'svelte';
  import { onDestroy } from 'svelte';
  //import Keypad from './Keypad.svelte';
  import Eliza from 'elizabot';
  import { beforeUpdate, afterUpdate } from 'svelte';

  // let
  let div
  let autoscroll
  let seconds = 0;
  const interval = setInterval(() => seconds += 1, 1000);

  let photos = [];
  let pin;
  $: view = pin ? pin.replace(/\d(?!$)/g, '•') : 'enter your pin';

  let canvas;
  let w;
	let h;
	let size = 42;
	let text = 'edit me';
  
  // function
  beforeUpdate(() => {
	autoscroll = div && (div.offsetHeight + div.scrollTop) > (div.scrollHeight - 20);
  });

  afterUpdate(() => {
	if (autoscroll) div.scrollTo(0, div.scrollHeight);
  });
  const eliza = new Eliza();

  let comments = [
		{ author: 'eliza', text: eliza.getInitial() }
	];

  function handleKeydown(event) {
		if (event.key === 'Enter') {
			const text = event.target.value;
			if (!text) return;

			comments = comments.concat({
				author: 'user',
				text
			});

			event.target.value = '';

			const reply = eliza.transform(text);

			setTimeout(() => {
				comments = comments.concat({
					author: 'eliza',
					text: '...',
					placeholder: true
				});

				setTimeout(() => {
					comments = comments.filter(comment => !comment.placeholder).concat({
						author: 'eliza',
						text: reply
					});
				}, 500 + Math.random() * 500);
			}, 200 + Math.random() * 200);
		}
	}

  onDestroy(() => clearInterval(interval));

//   function handleSubmit() {
// 		alert(`submitted ${pin}`);
// 	}
  
  onMount(async () => {
		const res = await fetch(`https://jsonplaceholder.typicode.com/photos?_limit=20`);
		photos = await res.json();
	});

  onMount(() => {
		const ctx = canvas.getContext('2d');
		let frame;

		(function loop() {
			frame = requestAnimationFrame(loop);

			const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);

			for (let p = 0; p < imageData.data.length; p += 4) {
				const i = p / 4;
				const x = i % canvas.width;
				const y = i / canvas.height >>> 0;

				const t = window.performance.now();

				const r = 64 + (128 * x / canvas.width) + (64 * Math.sin(t / 1000));
				const g = 64 + (128 * y / canvas.height) + (64 * Math.cos(t / 1000));
				const b = 128;

				imageData.data[p + 0] = r;
				imageData.data[p + 1] = g;
				imageData.data[p + 2] = b;
				imageData.data[p + 3] = 255;
			}

			ctx.putImageData(imageData, 0, 0);
		}());

		return () => {
			cancelAnimationFrame(frame);
		};
	});
  
</script>

<style>
  	.chat {
		display: flex;
		flex-direction: column;
		height: 100%;
		max-width: 320px;
	}

	.scrollable {
		flex: 1 1 auto;
		border-top: 1px solid #eee;
		margin: 0 0 0.5em 0;
		overflow-y: auto;
	}

	article {
		margin: 0.5em 0;
	}

	.user {
		text-align: right;
	}

	span {
		padding: 0.5em 1em;
		display: inline-block;
	}

	.eliza span {
		background-color: #eee;
		border-radius: 1em 1em 1em 0;
	}

	.user span {
		background-color: #0074D9;
		color: white;
		border-radius: 1em 1em 0 1em;
		word-break: break-all;
	}


  .photos {
		width: 100%;
		display: grid;
		grid-template-columns: repeat(5, 1fr);
		grid-gap: 8px;
	}

	figure, img {
		width: 100%;
		margin: 0;
	}

  canvas {
		width: 100%;
		height: 100%;
		background-color: #666;
		-webkit-mask: url(Svelte_Logo.svg) 50% 50% no-repeat;
		mask: url(Svelte_Logo.svg) 50% 50% no-repeat;
	}
	input { display: block; }
	div { display: inline-block; }
	span { word-break: break-all; }
</style>

<canvas
        bind:this={canvas}
	width={320}
	height={320}
></canvas>
    
<main>
  <div id="container">
    <div class="chat">
	<h1>Eliza</h1>

	<div class="scrollable" bind:this={div}>
		{#each comments as comment}
			<article class={comment.author}>
				<span>{comment.text}</span>
			</article>
		{/each}
	</div>

	<input on:keydown={handleKeydown}>
</div>

    <p>
	The page has been open for
	{seconds} {seconds === 1 ? 'second' : 'seconds'}
    </p>
    <h1>Photo album</h1>

<div class="photos">
	{#each photos as photo}
		<figure>
			<img src={photo.thumbnailUrl} alt={photo.title}>
			<figcaption>{photo.title}</figcaption>
		</figure>
	{:else}
		<!-- this block renders when photos.length === 0 -->
		<p>loading...</p>
	{/each}
</div>

<!--   <h1 style="color: {pin ? '#333' : '#ccc'}">{view}</h1>
<Keypad bind:value={pin} on:submit={handleSubmit}/-->

    <h1>Caminandes: Llamigos and canvas</h1>
<p>From <a href="https://cloud.blender.org/open-projects">Blender Open Projects</a>. CC-BY</p>

  </div>
</main>

