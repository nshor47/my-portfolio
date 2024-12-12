<script>
  import projects from '$lib/projects.json';
  import Project from '$lib/Project.svelte';
</script>


<svelte:head>
  <title>Nicholas Shor</title>
</svelte:head>

<h1>Nicholas Shor</h1>
<p>I am a motivated and accomplished individual with a strong foundation in data science, AI development, and analytical thinking. With a Bachelor’s degree in Data Science (GPA 3.74) and ongoing pursuit of a Master’s, I have honed my technical skills in AI, machine learning, and data analysis. My experience includes optimizing AI models, processing complex datasets, and collaborating on impactful projects. Additionally, my background as a D1 student-athlete has strengthened my dedication, teamwork, and leadership abilities. I am eager to apply my skills to tackle complex challenges and deliver meaningful solutions.</p>
<img src="images/Me.jpg" alt="Picture of me"/>


<section>
  <h2>GitHub Profile Stats</h2>
  
  {#await fetch("https://api.github.com/users/nshor47") }
    <p>Loading...</p>
    
  {:then response} 
    {#await response.json()}
      <p>Decoding...</p>
      
    {:then data}
      <dl class="stats-list">
        <dt>Public Repos</dt>
        <dd>{data.public_repos}</dd>

        <dt>Followers</dt>
        <dd>{data.followers}</dd>

        <dt>Following</dt>
        <dd>{data.following}</dd>

        <dt>Gists</dt>
        <dd>{data.public_gists}</dd>
      </dl>
      
    {:catch error}
      <p class="error">Something went wrong: {error.message}</p>
      
    {/await}
    
  {:catch error}
    <p class="error">Something went wrong: {error.message}</p>
    
  {/await}
</section>



<h2>Latest Projects</h2>
<div class="projects">
  {#each projects.slice(0, 3) as p}
    <Project data={p} hLevel=3 />
  {/each}
</div>

<style>
  .stats-list {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
  text-align: center;
}

.stats-list dt {
  grid-row: 1; 
  font-weight: bold;
}

.stats-list dd {
  grid-row: 2; 
  margin: 0;
}
</style>