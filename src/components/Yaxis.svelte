<script>
    export let yScale;
    export let height;
    export let hoveredData;
    import { fade } from "svelte/transition";
    let nations = yScale.domain();
</script>

 
<g class='axis-y'>
  {#each nations as nation}
    <g class='tick' in:fade={{delay: nation === "Afro-américain" ? yScale(nation)*6 : yScale(nation)*1.5}} >
     <text
        text-anchor='start'
        font-weight={hoveredData ? (hoveredData.nation === nation ? 900 : 300) : 300}
        font-size={hoveredData ? (hoveredData.nation === nation ? '18px' : '16px') : '16px'}
        dominant-baseline = 'middle'
        y={yScale(nation)} 
        x=-128
        dy=17>
        {nation}
    </text>
    </g>
  {/each}
  </g>
  <line
  x=0
  x2=0
  y=-1
  y2={height-60}
  stroke=#998c7e
  opacity=0.3
  >
</line>
  
  
  <style>

  </style>