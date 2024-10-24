<script>
    export let data = {};
    export let hLevel = 3;
</script>

<div class="projects">
    <svelte:element this={'h' + hLevel}>{data.title}</svelte:element>
    <article>
        <img src={data.image} alt={data.title}/>
        <p>{data.description}</p>
    </article>
</div>

<style>
    .projects{
        display:grid;
        grid-template-columns: repeat(auto-fill, minmax(26em, 1fr));
        gap: 1em;
    }

    .projects article {
        display: grid;
        grid-template-rows: subgrid;
        grid-row: span 3;
        background-color: var(--color-header-bg);
        padding: 15px; 
        border-radius: 8px;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    }
</style>