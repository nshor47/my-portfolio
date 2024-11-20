<script>
    export let data = [];
    export let hLevel = 3;
    import Pie from '$lib/Pie.svelte';
    import * as d3 from 'd3';
    import projects from '$lib/projects.json';
    let query = '';
</script>


<div class="projects">
    <article class="project">
        <svelte:element this={'h' + hLevel} class="project-title">{data.title}</svelte:element>
        <img src={data.image} alt={data.title}/>
        <div class="description"></div>
        <p>{data.description}</p>
        {#if data.url}
            <p>
                <a href={data.url} target="_blank" rel="noopener noreferrer">
                    Visit the project website
                </a>
            </p>
        {/if}
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
</style>


