<script>
    export let data = [];
    export let hLevel = 3;
    import Pie from '$lib/Pie.svelte';
    import * as d3 from 'd3';
    import projects from '$lib/projects.json';

    let query = '';

    let rolledData = d3.rollups(
    projects,
    (v) => v.length,
    (d) => d.year,
    );

    let pieData = rolledData.map(([year, count]) => {
    return { value: count, label: year };
    });


    let colors = d3.scaleOrdinal(d3.schemeTableau10);

    let sliceGenerator = d3.pie().value((d) => d.value);    
    let arcData = sliceGenerator(pieData);
    let arcGenerator = d3.arc().innerRadius(0).outerRadius(50);  

    let filteredProjects;
    $: filteredProjects = projects.filter((project) => {
        if (query) {
            return project.title.toLowerCase().includes(query.toLowerCase());
        }

        return true;
        });
    $: filteredProjects = projects.filter((project) => {
        let values = Object.values(project).join('\n').toLowerCase();
        return values.includes(query.toLowerCase());
        });
</script>

<input
  type="search"
  bind:value="{query}"
  aria-label="Search projects"
  placeholder="🔍 Search projects…"
/>

<div class="container">
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
</div>

<div class="projects">
    <article class="project">
        <svelte:element this={'h' + hLevel} class="project-title">{data.title}</svelte:element>
        <img src={data.image} alt={data.title}/>
        <div class="description"></div>
        <p>{data.description}</p>
        <p>c. {data.year}</p>
    </article>
</div>

<!-- <div class="projects">
    {#each filteredProjects as project}
        <article class="project">
            <svelte:element this={'h' + hLevel} class="project-title">{project.title}</svelte:element>
            <img src={project.image} alt={project.title}/>
            <div class="description"></div>
            <p>{project.description}</p>
            <p>c. {project.year}</p>
        </article>
    {/each}
</div> -->

<style>
    .projects {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(550px, 1fr));
        gap: 1.5em;
        justify-content: center; 
        padding: 2em; 
        max-width: 1000px; 
        margin: 0 auto;
    }

    .projects article {
        display: flex;
        flex-direction: column;
        align-items: center;
        background-color: var(--color-header-bg);
        padding: 15px;
        border-radius: 8px;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        text-align: center; 
    }

    .projects img {
        width: 100%; 
        max-width: 700px; 
        height: auto;
        margin-bottom: 1em;
        border-radius: 8px;
    }

    .project-title {
        font-size: 2em;
    }
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


