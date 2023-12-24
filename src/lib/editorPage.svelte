<script lang="ts">
  import ColorDisplay from './colorDisplay.svelte';
  import type { Details } from './details';
  import Slider from './slider.svelte';

  let details: Details = {
    red: 0.5,
    blue: 0.9,
    green: 0.2,
  };
</script>

<div class="flex-1 items-center justify-center">
  <ColorDisplay {details} />
  <div class="w-full max-w-md">
    <div class="grid" style="grid-template-columns: auto 1fr;">
      <div>red</div>
      <div class="flex-1 justify-center" style="--channel-color:red;">
        <Slider bind:value={details.red} min={0} max={1} />
      </div>
      <div>green</div>
      <div class="flex-1 justify-center" style="--channel-color:green;">
        <Slider bind:value={details.green} min={0} max={1} />
      </div>
      <div>blue</div>
      <div class="flex-1 justify-center" style="--channel-color:blue;">
        <Slider bind:value={details.blue} min={0} max={1} />
      </div>
    </div>
  </div>
</div>
