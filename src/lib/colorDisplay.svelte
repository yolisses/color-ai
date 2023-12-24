<script lang="ts">
  import type { Details } from './details';

  export let details: Details;

  function getByte(value: number) {
    return Math.round(value * 255);
  }

  function getRGBColor(details: Details) {
    const { red, green, blue } = details;
    return `rgb(${getByte(red)}, ${getByte(green)}, ${getByte(blue)})`;
  }
</script>

<div class="w-40 aspect-square rounded-lg" style="background-color: {getRGBColor(details)};" />
