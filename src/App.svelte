<script>
  // import
  import { onMount } from 'svelte';
  import Keypad from './Keypad.svelte';

  // let
  let pin;
  $: view = pin ? pin.replace(/\d(?!$)/g, '•') : 'enter your pin';

  let canvas;
  let w;
	let h;
	let size = 42;
	let text = 'edit me';
  
  // function
  function handleSubmit() {
		alert(`submitted ${pin}`);
	}
  
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
   <h1 style="color: {pin ? '#333' : '#ccc'}">{view}</h1>

<Keypad bind:value={pin} on:submit={handleSubmit}/>

    <h1>Caminandes: Llamigos and canvas</h1>
<p>From <a href="https://cloud.blender.org/open-projects">Blender Open Projects</a>. CC-BY</p>

  </div>
</main>

