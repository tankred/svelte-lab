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
  let time = 0;
	let duration;
	let paused = true;
  	let showControls = true;
	let showControlsTimeout;
  let todos = [
		{ done: false, text: 'finish Svelte tutorial' },
		{ done: false, text: 'build an app' },
		{ done: false, text: 'make the world a better place' }
  ];
  let html = '<p>Write some text!</p>';
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
  function handleMousemove(e) {
		// Make the controls visible, but fade out after
		// 2.5 seconds of inactivity
		clearTimeout(showControlsTimeout);
		showControlsTimeout = setTimeout(() => showControls = false, 2500);
		showControls = true;

		if (!(e.buttons & 1)) return; // mouse not down
		if (!duration) return; // video not loaded yet

		const { left, right } = this.getBoundingClientRect();
		time = duration * (e.clientX - left) / (right - left);
	}

	function handleMousedown(e) {
		// we can't rely on the built-in click event, because it fires
		// after a drag — we have to listen for clicks ourselves

		function handleMouseup() {
			if (paused) e.target.play();
			else e.target.pause();
			cancel();
		}

		function cancel() {
			e.target.removeEventListener('mouseup', handleMouseup);
		}

		e.target.addEventListener('mouseup', handleMouseup);

		setTimeout(cancel, 200);
	}

	function format(seconds) {
		if (isNaN(seconds)) return '...';

		const minutes = Math.floor(seconds / 60);
		seconds = Math.floor(seconds % 60);
		if (seconds < 10) seconds = '0' + seconds;

		return `${minutes}:${seconds}`;
	}
  function add() {
		todos = todos.concat({ done: false, text: '' });
  }
  function clear() {
		todos = todos.filter(t => !t.done);
  }
  $: remaining = todos.filter(t => !t.done).length;
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
    
    <h1>Caminandes: Llamigos</h1>
<p>From <a href="https://cloud.blender.org/open-projects">Blender Open Projects</a>. CC-BY</p>

<div>
	<video
		poster="https://sveltejs.github.io/assets/caminandes-llamigos.jpg"
		src="https://sveltejs.github.io/assets/caminandes-llamigos.mp4"
		on:mousemove={handleMousemove}
		on:mousedown={handleMousedown}
	></video>

	<div class="controls" style="opacity: {duration && showControls ? 1 : 0}">
		<progress value="{(time / duration) || 0}"/>

		<div class="info">
			<span class="time">{format(time)}</span>
			<span>click anywhere to {paused ? 'play' : 'pause'} / drag to seek</span>
			<span class="time">{format(duration)}</span>
		</div>
	</div>
</div>

  </div>
</main>

<style>
  body {
    background-color: #0f0; 
  }
  div {
		position: relative;
	}

	.controls {
		position: absolute;
		top: 0;
		width: 100%;
		transition: opacity 1s;
	}

	.info {
		display: flex;
		width: 100%;
		justify-content: space-between;
	}

	span {
		padding: 0.2em 0.5em;
		color: white;
		text-shadow: 0 0 8px black;
		font-size: 1.4em;
		opacity: 0.7;
	}

	.time {
		width: 3em;
	}

	.time:last-child { text-align: right }

	progress {
		display: block;
		width: 100%;
		height: 10px;
		-webkit-appearance: none;
		appearance: none;
	}

	progress::-webkit-progress-bar {
		background-color: rgba(0,0,0,0.2);
	}

	progress::-webkit-progress-value {
		background-color: rgba(255,255,255,0.6);
	}

	video {
		width: 100%;
	}
  #container {
    margin: 1.24em;
    display: grid;
    grid-gap: 1.53rem;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  }
  .done {
		opacity: 0.4;
  }
  [contenteditable] {
		padding: 0.5em;
		border: 1px solid #eee;
		border-radius: 4px;
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
