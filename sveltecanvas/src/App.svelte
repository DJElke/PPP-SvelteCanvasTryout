<script>
	import { onMount } from 'svelte'

	let width = 960;
	let height = 640;
	let canvas, context, img, imageData, showImage;

	onMount(() => {
		img = new Image();
		img.src = "/images/cat.png";

		canvas = document.querySelector('.canvas');
		context = canvas.getContext('2d');

		img.addEventListener('load', () => {
			context.drawImage(img, 0,0);
		});

		context.fillCircle = (x,y,radius) => {
            context.beginPath();
            context.moveTo(x, y);
            context.arc(x, y, radius, 0, Math.PI * 2, false);
            context.fill();
		}		
	});

	const mouseDown = () => {
		console.log("down");
		canvas.isDrawing = true;
	}

	const mouseUp = () => {
		console.log("up");
		canvas.isDrawing = false;
	}

	const erase = (e) => {
		if(!canvas.isDrawing){
			return;
		}
		let x = e.pageX - canvas.offsetLeft;
		let y = e.pageY - canvas.offsetTop;
		let radius = 10;
		context.globalCompositeOperation = 'destination-out';
		context.fillCircle(x,y,radius);
	}

	const showDrawing = () => {
		imageData = canvas.toDataURL('image/png');
		showImage = document.querySelector('.drawing');
		showImage.classList.remove('display-none');
		showImage.src = imageData;
	};
</script>

<main>
	<canvas on:mousedown={mouseDown} on:mouseup={mouseUp} on:mousemove={erase} class="canvas" width={width} height={height}></canvas>
	<button on:click={showDrawing}>save drawing</button>
	<img class="drawing display-none" alt="drawing"/>
</main>

<style>
	.display-none{
		display: none;
	}

	canvas{
		background: transparent;
	}
</style>

