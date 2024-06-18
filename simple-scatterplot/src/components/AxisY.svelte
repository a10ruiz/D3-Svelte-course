<script>
    export let yScale;
    export let width;
    let yTicks = yScale.ticks(4); // utilizamos la funcion ticks de scale que te da ticks de forma automatica (no siempre exactamente el numero que pones, pero lo intenta en base a los datos que tienens)
</script>

<g>
    {#each yTicks as tick, index}
        <g class="tick" transform="translate(0, {yScale(tick)})">
            <line
                x1="0"
                y1="0"
                y2="0"
                x2={width}
                stroke={index == 0 ? "#8f8f8f" : "#e5e7eb"}
            ></line>
            <text x="0" y="-4"> {tick}
                {index == yTicks.length - 1? " hours studied" : ""}</text>
        </g>
    {/each}
</g>
