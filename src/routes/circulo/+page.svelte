<script lang="ts">
  import { onMount, onDestroy } from 'svelte';

  let canvas: HTMLCanvasElement;
  let ctx: CanvasRenderingContext2D;

  let animationFrame: number;

  const baseColors = [
    '#9caf88', // sage green
    '#7b9453', // olive
    '#d1b253', // mustard
    '#d17842', // burnt orange
    '#882b3f'  // wine red
  ];
  let colors = [...baseColors];

  let gridSize = 64;
  let pixelSize = 10; // Se ajustará dinámicamente

  function resizeCanvas() {
    const size = Math.min(window.innerWidth, window.innerHeight) * 0.9;
    canvas.width = size;
    canvas.height = size;

    pixelSize = size / gridSize;

    draw();
  }

  function draw() {
    if (!ctx) return;

    const center = gridSize / 2;
    const radius = center;

    ctx.clearRect(0, 0, canvas.width, canvas.height);

    ctx.save();
    ctx.beginPath();
    ctx.arc(canvas.width / 2, canvas.height / 2, radius * pixelSize, 0, Math.PI * 2);
    ctx.clip();

    for (let y = 0; y < gridSize; y++) {
      for (let x = 0; x < gridSize; x++) {
        const dx = x - center;
        const dy = y - center;
        const dist = Math.sqrt(dx * dx + dy * dy);

        if (dist < radius) {
          const ring = Math.floor((dist / radius) * colors.length);
          ctx.fillStyle = colors[ring] || '#000';

          // Centrado en el canvas
          const px = x * pixelSize + (canvas.width - gridSize * pixelSize) / 2;
          const py = y * pixelSize + (canvas.height - gridSize * pixelSize) / 2;
          ctx.fillRect(px, py, pixelSize, pixelSize);
        }
      }
    }

    ctx.restore();
  }

  function animate() {
    colors.unshift(colors.pop());
    draw();
    animationFrame = requestAnimationFrame(animate);
  }

  onMount(() => {
    ctx = canvas.getContext('2d')!;
    resizeCanvas();
    window.addEventListener('resize', resizeCanvas);
    animationFrame = requestAnimationFrame(animate);
  });

  onDestroy(() => {
    cancelAnimationFrame(animationFrame);
    window.removeEventListener('resize', resizeCanvas);
  });
</script>

<canvas bind:this={canvas} class="responsive-canvas" />

<style>
  .responsive-canvas {
    display: block;
    margin: auto;
    background: transparent;
    border: none;
    image-rendering: auto;
    width: 100vw;
    height: 100vh;
  }
  body {
    margin: 0;
    background: #111;
  }
</style>
