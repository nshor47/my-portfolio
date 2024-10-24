<script>
  import projects from '$lib/projects.json';
  import Project from '$lib/Project.svelte';
</script>


<svelte:head>
  <title>Nicholas Shor</title>
</svelte:head>

<h1>Nicholas Shor</h1>
<p>I am a first year graduate student studying Data Science at UCSD. I love soccer and have played it my entire life.</p>
<img src="images/Me.jpg" alt="Picture of me"/>

<h2>Latest Projects</h2>
<div class="projects">
  {#each projects.slice(0, 3) as p}
    <Project data={p} hLevel=3 />
  {/each}
</div>

