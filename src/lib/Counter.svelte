<script lang="ts">
  import { Canvas, Layer } from 'svelte-canvas';
  import biweeklyEn from '../assets/biweekly-en.png';
  import biweeklyZh from '../assets/biweekly-zh.png';

  const size = {
    width: 768,
    height: 407,
  };

  $: dateStr = '2022.6.1-2022.6.15';

  $: renderEn = async ({ context: ctx }) => {
    const image = new Image();
    image.src = biweeklyEn;
    // wait image loaded
    await new Promise<void>((resolve) => image.addEventListener('load', () => resolve()));

    ctx.drawImage(image, 0, 0);

    ctx.font = '23px system-ui';
    ctx.fillStyle = '#510dae';
    ctx.fillText(dateStr, 511, 340);
  };

  $: renderZh = async ({ context: ctx }) => {
    const image = new Image();
    image.src = biweeklyZh;
    // wait image loaded
    await new Promise<void>((resolve) => image.addEventListener('load', () => resolve()));

    ctx.drawImage(image, 0, 0);

    ctx.font = '23px system-ui';
    ctx.fillStyle = '#510dae';
    ctx.fillText(dateStr, 429, 180);
  };

  let canvasZh, canvasEn;

  const download = () => {
    [canvasEn, canvasZh].forEach((v, i) => {
      const canvasUrl = v.getCanvas().toDataURL('image/png');
      console.log(canvasUrl, v.canvas);
      const createEl = document.createElement('a');
      createEl.href = canvasUrl;
      createEl.download = `biweekly-${dateStr}-${i}.png`;
      createEl.click();
      createEl.remove();
    });
  };
</script>

<menu>
  <input bind:value={dateStr} placeholder="enter date range" />
  <button on:click={download}>Download Images</button>
</menu>

<Canvas bind:this={canvasEn} {...size} style="background-color: #666;">
  <Layer render={renderEn} />
</Canvas>
<br />
<Canvas bind:this={canvasZh} {...size} style="background-color: #666;">
  <Layer render={renderZh} />
</Canvas>

<style>
  menu {
    display: flex;
    flex-direction: row;
    align-items: center;
    font-size: 1.25rem;
    margin: 0;
    padding: 0;
    margin-bottom: 1.5rem;
  }

  input,
  button {
    font-family: inherit;
    font-size: inherit;
    -webkit-padding: 0.4em 0;
    padding: 0.4em;
    margin: 0 1.5em 0 0;
    box-sizing: border-box;
    border: 1px solid #ccc;
    border-radius: 10px;
  }

  button {
    background: linear-gradient(45deg, #ff6d1b, #e0417f);
    padding: 0.75rem 1.5rem;
    line-height: 1.5rem;
    will-change: transform, filter;
    margin: 0;
    transition: all 0.15s ease-out;
    cursor: pointer;
    border-radius: 100px;
    display: block;
    display: block;
    border: none;
    color: white;
    font-family: Inter, sans-serif;
    /* filter: drop-shadow(0 6px 12px rgba(0, 0, 0, 0.3)); */
    font-variation-settings: 'wght' 600;
    perspective-origin: 0 0;
    letter-spacing: 0;
  }

  button:hover {
    transform: scale(1.04);
  }
</style>
