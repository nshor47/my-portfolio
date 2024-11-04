<script>
    export let data = {};
    export let hLevel = 3;
</script>

<div class="projects">
    <article class="project">
        <svelte:element this={'h' + hLevel} class="project-title">{data.title}</svelte:element>
        <img src={data.image} alt={data.title}/>
        <div class="description"></div>
        <p>{data.description}</p>
        <p>c. {data.year}</p>
    </article>
</div>

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
</style>


