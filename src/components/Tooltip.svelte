<script>
    export let data;
    export let xScale; 
    export let yScale;
    export let margin;

let lab = Math.round(data.score * 10) / 10;

$: nudge = data.nation === "Suéde" ? -5 : 60;

$: x = xScale(data.score) 
$: y = yScale(data.nation) + 3
$: xPosition = x + margin.left - nudge;
$: yPosition = y + margin.top;
</script>


<div 
class='tooltip'
style="
    left: {xPosition}px; 
    top:{yPosition}px;
    color: {data.nation === "Suéde" ? "grey" : "#d8cec2"};
    "
>
    <p>{lab}%</p>
</div>

<style>
    .tooltip {
        font-weight: 600;
        position: absolute;
        text-align: left;
        padding: 8px;
        border-radius: 3px;
        pointer-events: none;
    }
</style>

