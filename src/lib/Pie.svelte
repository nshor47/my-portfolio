<script>
    import * as d3 from 'd3';
    export let data = [];
    export let selectedIndex = -1;

    // Set up colors
    let colors = d3.scaleOrdinal(d3.schemeTableau10);

    // Arc and pie generators
    let sliceGenerator = d3.pie().value(d => d.value);
    let arcGenerator = d3.arc().innerRadius(0).outerRadius(50);

    // Generate arc data
    $: arcData = sliceGenerator(data);

    // Get the label of the selected year (if any)
    $: selectedYear = selectedIndex > -1 ? data[selectedIndex].label : null;

    // Toggle function for slice selection
    function toggleWedge(index, event) {
        if (!event.key || event.key === 'Enter') {
            selectedIndex = selectedIndex === index ? -1 : index;
        }
    }
</script>

<!-- SVG for the pie chart -->
<div class="container">
    <svg viewBox="-50 -50 100 100">
        {#each arcData as arc, i}
            <path
                d={arcGenerator(arc)}
                fill={colors(i)}
                tabindex="0"
                role="button"
                aria-label="Select {data[i].label} slice"
                class:selected={selectedIndex === i}
                on:click={e => toggleWedge(i, e)}
                on:keyup={e => toggleWedge(i, e)}
            />
        {/each}
    </svg>

    <!-- Legend -->
    <ul class="legend">
        {#each data as d, index}
            <li 
                class:selected={selectedIndex === index} 
                style="--color: { colors(index) }"
            >
                <span class="swatch"></span>
                {d.label} <em>({d.value})</em>
            </li>
        {/each}
    </ul>
</div>

<style>
    svg {
        max-width: 20em;
        margin-block: 2em;
        overflow: visible;
    }

    :root {
        --swatch-border-color: #5f5f5f; 
    }

    :root[data-theme='dark'] {
        --swatch-border-color: #fff; 
    }

    .legend {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(9em, 1fr));
        gap: 0.5em;
        margin: 1em 0;
        list-style: none;
        padding: 0;
        border: 1.2px solid var(--swatch-border-color);
    }

    .legend li {
        display: flex;
        align-items: center;
        gap: 0.5em;
    }

    .swatch {
        width: 1em;
        height: 1em;
        aspect-ratio: 1 / 1;
        background-color: var(--color);
        border-radius: 50%;
        display: inline-block;
        border: 1px solid #3f3e3e;
    }

    .container {
        display: flex;
        align-items: center;
        gap: 1em;
        flex-wrap: wrap;
    }

    .selected {
        --color: oklch(60% 45% 0) !important;
    }

    .selected:is(path) {
        fill: var(--color);
    }

    svg:has(path:hover, path:focus-visible)  {
        opacity: 100%;
    }

    path:not(:hover, :focus-visible) {
        opacity: 65%;
    }

    path {
        cursor: pointer;
        transition: 300ms;
        outline: none;
    }

    path:focus-visible {
        opacity: 100%;
        outline: 3px solid #5c6bc0;
    }
</style>