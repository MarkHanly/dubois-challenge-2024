<script>

    export let xScale;
    export let height;
    export let width;

    $: ticks = [0, 25, 50, 75];
</script>

<g class='axis-x'>
  {#each ticks as tick}
  <g class='tick' transform="translate({xScale(tick)}, 0)">
    <text 
      x="6" 
      y={height+16}
      dominant-baseline = 'auto'
      dx={tick===100 ? -40 : 5}
      >{tick}
    </text>
    <line x1=0 x2=0 y1=0 y2={height} stroke='#D3D3D3'></line>
  </g>
{/each}
<text 
  x={480} 
  y={height} 
  class='axis-title' 
  text-anchor="end" 
  dominant-baseline = 'hanging' 
  dy=20>
  Literacy &rarr;</text>
</g>

<style>

  .tick {
    color: red;
  }

</style>
