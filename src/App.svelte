<script>
	import Moveable from "svelte-moveable";
	const frame = {
		translate: [0, 0],
	};
	let target;
	let src = "../SmileyBois.jpg";
</script>

<div class="target" bind:this={target}>
	<h3>Aiki</h3>
	<p>This is the aiki window</p>
	<button>Emergency Skip</button>
	<button>Continue</button>
	<img {src} alt="hey">
</div>
<Moveable
    draggable={true}
    target={target}
    throttleDrag={0}
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
/>

<style>

	.target {
		size: 50px 50px;
		position: fixed;
		top: 0px;
		background-color: rgba(200,0,0,0.1);
	}

</style>