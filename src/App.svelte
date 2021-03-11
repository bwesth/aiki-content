<script>
	import Moveable from "svelte-moveable";
	const frame = {
		translate: [0, 0],
	};

	let target;

	let barWidth = 0;
	let progress;
	let targetTime = 100;

	$: if (barWidth === targetTime) {
		clearInterval(progress);
	}

	$: if (barWidth > 49) {
		document.getElementById("myBar").style.backgroundColor="green";
	}

	const addColour = () => barWidth += 1;

	const makeProgress = () => {
		progress = setInterval(addColour,100);
	}

	makeProgress();

</script>

<div class="target" bind:this={target}>
	<h3>Aiki</h3>
	<p>You are {barWidth}% complete with your task.</p>

	<div id="myProgress">
		<div id="myBar" style="width:{barWidth}%"></div>
	</div>

	{#if barWidth===targetTime}
	<p>Congrats! You're done!</p>
	<button class="continue">Continue</button>
	{:else if barWidth < targetTime}
	<p>You can skip if you want.</p>
	<button class="emergencySkip">Emergency Skip</button>
	{/if}
	<br>
	
</div>

<Moveable
	origin={false}
	dragTarget={false}
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

	#myProgress {
		width: 100%;
		background-color: grey;
		border-radius: 10px;
	}

	#myBar {
		width: 0%;
		height: 30px;
		background-color: red;
		border-radius: 10px;
	}

	.target {
		padding: 20px;
		width: 300px;
		position: fixed;
		z-index: 1000;
		top: 150px;
		left: 150px;
		background-color: whitesmoke;
		border: 3px solid grey;
		border-radius: 10px;
	}

	button {
		border-radius: 10px;
		padding:8px;
	}

	.emergencySkip {
		background-color: red;
		color: white;
	}

	.continue {
		background-color: green;
		color: white;
	}

</style>