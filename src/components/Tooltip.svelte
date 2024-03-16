<script>
    export let data;
    export let xScale; 
    export let yScale;
    export let margin;

let lab = Math.round(data.score * 10) / 10;

$: nudge = data.nation === "Suéde" || data.nation === "France" || data.nation === "Ireland" ? -5 : 60;

$: x = xScale(data.score) 
$: y = yScale(data.nation)
$: xPosition = x + margin.left - nudge;
$: yPosition = y + margin.top;
</script>


<div 
class='tooltip'
style="
    left: {xPosition}px; 
    top:{yPosition -2}px;
    color: {data.nation === "Suéde" || data.nation === "France" || data.nation === "Ireland" ? "grey" : "#d8cec2"};
    "
>
    <p>{lab}%</p>
</div>

<style>
    .tooltip {
        font-weight: 600;
        position: absolute;
        padding: 8px;
        border-radius: 3px;
        pointer-events: none;
    }
</style>

