<script>
    import projects from '$lib/projects.json';
    import Project from '$lib/Project.svelte';
    import Pie from '$lib/Pie.svelte';

    let pieData = [
    { value: 1, label: 'apples' },
    { value: 2, label: 'oranges' },
    { value: 3, label: 'mangos' },
    { value: 4, label: 'pears' },
    { value: 5, label: 'limes' },
    { value: 5, label: 'cherries' },
    ];
    
</script>
<Pie data="{pieData}" />
<svelte:head>
  <title>My Projects</title>
</svelte:head>
<h1>My Projects</h1>
    <div class="projects">
        {#each projects as p}
            <Project data={p} />
        {/each}
    </div>
