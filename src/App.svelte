<script>
  // import
  
  // let
  let w;
	let h;
	let size = 42;
	let text = 'edit me';
  
  // function
  
</script>

<style>
	input { display: block; }
	div { display: inline-block; }
	span { word-break: break-all; }
</style>

<main>
  <div id="container">
    
    <h1>Caminandes: Llamigos</h1>
<p>From <a href="https://cloud.blender.org/open-projects">Blender Open Projects</a>. CC-BY</p>

<input type=range bind:value={size}>
<input bind:value={text}>

<p>size: {w}px x {h}px</p>

<div bind:clientWidth={w} bind:clientHeight={h}>
	<span style="font-size: {size}px">{text}</span>
</div>
  </div>
</main>

