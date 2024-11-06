<script>
    import projects from '$lib/projects.json';
    import Project from '$lib/Project.svelte';
    import * as d3 from 'd3';
</script>

<!-- <div class="container">
    <svg viewBox="-50 -50 100 100">
        {#each arcData as arc, i}
            <path d={arcGenerator(arc)} fill={colors(i)} />
        {/each}
    </svg>

    <ul class="legend">
        {#each pieData as d, index}
            <li style="--color: { colors(index) }">
                <span class="swatch"></span>
                {d.label} <em>({d.value})</em>
            </li>
        {/each}
    </ul>
</div> -->

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
        flex:1;
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
</style>
