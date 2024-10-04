<script>
    import { onMount } from 'svelte';
    let canvas;
    let ctx;
    let mouse = { x: 0, y: 0 };

    onMount(() => {
        ctx = canvas.getContext('2d');
        resizeCanvas();
        window.addEventListener('resize', resizeCanvas);

        addEventListener('mousemove', (event) => {
            mouse.x = event.clientX;
            mouse.y = event.clientY;
        });

        requestAnimationFrame(animate);
    });

    function resizeCanvas() {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
    }

    function animate() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        requestAnimationFrame(animate);
    }
</script>

<style>
    canvas {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -10;

    }
</style>

<canvas bind:this={canvas}></canvas>
