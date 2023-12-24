<script lang="ts">
  import { clamp } from './clamp';

  export let value: number;
  export let onInput: ((value: number) => void) | undefined = undefined;
  export let onChange: ((value: number) => void) | undefined = undefined;
  export let min = 0;
  export let max = 1;

  let active = false;
  let pointerId: number;
  let element: HTMLDivElement;
  let initialOffset = 0;
  let threshold = 10;

  $: range = max - min;
  $: width = clamp((value - min) / range, 0, 1);

  function updateValue(offsetX: number) {
    value = offsetX / element.clientWidth;
    value *= range;
    value += min;
    value = clamp(value, min, max);
    if (onInput) {
      onInput(value);
    }
  }

  function handlePointerDown(e: PointerEvent) {
    if (e.pointerType === 'touch') {
      initialOffset = e.offsetX;
      return;
    }

    pointerId = e.pointerId;
    element.setPointerCapture(pointerId);
    active = true;
    updateValue(e.offsetX);
  }

  function handlePointerMove(e: PointerEvent) {
    if (e.pointerType === 'touch') {
      const distance = Math.abs(e.offsetX - initialOffset);
      if (distance > threshold) {
        pointerId = e.pointerId;
        element.setPointerCapture(pointerId);
        active = true;
      }
    }
    if (!active) return;
    updateValue(e.offsetX);
  }

  function handlePointerUp(e: PointerEvent) {
    element.releasePointerCapture(pointerId);
    active = false;
    updateValue(e.offsetX);
    if (onChange) {
      onChange(value);
    }
  }
</script>

<div
  class="mx-2 touch-pan-y"
  bind:this={element}
  data-active={active}
  on:pointerup={handlePointerUp}
  on:pointerdown={handlePointerDown}
  on:pointermove={handlePointerMove}
>
  <div
    class="h-2 bg-white/20 my-1 flex-row items-center rounded-full relative pointer-events-none touch-none"
  >
    <div class="h-2 absolute left-0 top-0 colorful rounded-full" style="width: {width * 100}%;" />
    <div class="absolute w-0 h-0" style="left: {width * 100}%;">
      <div class="w-3 h-3 rounded-full absolute -translate-y-1/2 -translate-x-1/2 bg-white" />
    </div>
  </div>
</div>

<style lang="postcss">
  [data-active='true'] {
    @apply cursor-col-resize;
  }

  [data-active='false'] {
    @apply cursor-pointer;
  }
</style>
