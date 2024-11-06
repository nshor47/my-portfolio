<script>
    import projects from '$lib/projects.json';
    import Project from '$lib/Project.svelte';
    import Pie from '$lib/Pie.svelte';
    import * as d3 from 'd3';

    let query = '';
    let selectedYearIndex = -1;

    $: filteredProjects = projects.filter((project) => {
        let values = Object.values(project).join('\n').toLowerCase();
        return values.includes(query.toLowerCase());
    });

    let pieData;
    $: {
        let rolledData = d3.rollups(
            filteredProjects,
            (v) => v.length,
            (d) => d.year,
        );
        pieData = rolledData.map(([year, count]) => ({
            value: count,
            label: year,
        }));
    }

    let colors = d3.scaleOrdinal(d3.schemeTableau10);

    let sliceGenerator = d3.pie().value((d) => d.value); 
    let arcGenerator = d3.arc().innerRadius(0).outerRadius(50);  

    let arcData;
    $: arcData = sliceGenerator(pieData);

    export let selectedIndex = -1;
    let selectedYear;
    $: selectedYear = selectedYearIndex > -1 ? pieData[selectedYearIndex].label : null;

    $: filteredByYear = filteredProjects.filter((project) => {
        if (selectedYear) return project.year === selectedYear;
        return true;
    });
    function toggleWedge(index, event) {
    if (!event.key || event.key === 'Enter') {
        selectedIndex = selectedIndex === index ? -1 : index;
    }
}
</script>
{selectedYear}
<div class="container">
    <svg viewBox="-50 -50 100 100">
        {#each arcData as arc, i}
        <path
            d={arcGenerator(arc)}
            fill={colors(i)}
            tabindex="0"
            role="button"
            aria-label="Select {pieData[i].label} slice"
            class:selected={selectedIndex === i}
            on:click={e => toggleWedge(i, e)}
            on:keyup={e => toggleWedge(i, e)}
        />
{/each}
    </svg>

    <ul class="legend">
        {#each pieData as d, index}
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



<svelte:head>
  <title>My Projects</title>
</svelte:head>
<h1>My Projects</h1>
<input
    type="search"
    bind:value="{query}"
    aria-label="Search projects"
    placeholder="🔍 Search projects…"
/>

<div class="projects">
    {#each filteredProjects as p}
        <Project data={p} />
    {/each}
</div>

<Pie data="{pieData}" bind:selectedIndex="{selectedYearIndex}"/>


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
        gap: 0em;
        margin: 1em 0;
        list-style: none;
        padding: 10;
        border: 1.2px solid var(--swatch-border-color);
        flex: 1;
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