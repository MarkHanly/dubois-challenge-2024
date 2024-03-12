<script>
    export let yScale;
    export let height;
    export let hoveredData;
    let nations = yScale.domain();
</script>

<!--
  {nation === hoveredData.nation ? 800 : 300}
  {nation === hoveredData.nation ? '16.5px' : '16px'}
-->
<g class='axis-y'>
  {#each nations as nation}
    <g class='tick'>
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