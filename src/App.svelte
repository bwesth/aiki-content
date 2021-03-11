<script>
	export let name;
	import Moveable from "svelte-moveable";
	const frame = {
		translate: [0, 0],
	};
	let target;
</script>

<main>
	<h1>Hello {name}!</h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
</main>

<div class="target" bind:this={target}>Target</div>
<Moveable
    draggable={true}
	resizable={true}
    target={target}
    throttleDrag={0}
	throttleResize={0}
    on:dragStart={({ detail: { set } }) => {
        set(frame.translate)
    }}
    on:drag={({ detail: { target, beforeTranslate }}) => {
        frame.translate = beforeTranslate;
        target.style.transform
            = `translate(${beforeTranslate[0]}px, ${beforeTranslate[1]}px)`;
    }}
    on:dragEnd={({ detail: { target, isDrag, clientX, clientY }}) => {
        console.log("onDragEnd", target, isDrag);
    }}
	 
    on:resizeStart={({ detail: {target, set, setOrigin, dragStart }}) => {
        // Set origin if transform-orgin use %.
        setOrigin(["%", "%"]);

        // If cssSize and offsetSize are different, set cssSize. (no box-sizing)
        const style = window.getComputedStyle(target);
        const cssWidth = parseFloat(style.width);
        const cssHeight = parseFloat(style.height);
        set([cssWidth, cssHeight]);

        // If a drag event has already occurred, there is no dragStart.
        dragStart && dragStart.set(frame.translate);
    }}
    on:resize={({ detail: { target, width, height, drag }}) => {
        target.style.width = `${width}px`;
        target.style.height = `${height}px`;

        // get drag event
        frame.translate = drag.beforeTranslate;
        target.style.transform
            = `translate(${drag.beforeTranslate[0]}px, ${drag.beforeTranslate[1]}px)`;
    }}
    on:resizeEnd={({ detail: { target, isDrag, clientX, clientY }}) => {
        console.log("onResizeEnd", target, isDrag);
    }}
/>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

	.target {
		size: 50px 50px;
		position: fixed;
		top: 0px;
	}

</style>