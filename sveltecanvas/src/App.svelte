<script>
	import { onMount } from 'svelte'

	let width = window.innerWidth;
	let height = window.innerHeight;

	let canvas, context, img;

	let fillColor = "#ffffff";

	onMount(() => {
		img = new Image();
		img.src = "/images/cat.png";

		canvas = document.querySelector('.canvas');
		context = canvas.getContext('2d');

		img.addEventListener('load', () => {
			context.drawImage(img, 0,0);
		});

		context.fillCircle = (x,y,radius, fillColor) => {
			context.fillStyle = fillColor;
            context.beginPath();
            context.moveTo(x, y);
            context.arc(x, y, radius, 0, Math.PI * 2, false);
            context.fill();
		}

		// context.clearTo = (fillColor) => {
		// 	context.fillStyle = fillColor;
        //     context.fillRect(0, 0, width, height);
		// }

		// context.clearTo(fillColor || "#ffffff");

		
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
		fillColor = '#ff0000';
		context.globalCompositeOperation = 'destination-out';
		context.fillCircle(x,y,radius, fillColor);
	}
</script>

<main>
	<canvas on:mousedown={mouseDown} on:mouseup={mouseUp} on:mousemove={erase} class="canvas" width={width} height={height}></canvas>
</main>

