<nav>
    {#each pages as p}
        <a href={p.url} 
        target={p.url.startsWith("http") ? "_blank" : "_self"} 
        class:current={'.' + $page.route.id === p.url}>
            {p.title}
        </a>
        
    {/each}
</nav>

<script>
    import { page } from '$app/stores';
    import '../style.css';
    let pages = [
        {url: './', title: 'Home' },
        {url: './projects', title: 'Projects' },
        {url: './contact', title:'Contact' },
        {url: './resume', title: 'Resume'},
        {url: 'https://github.com/nshor47', title:'Github', target: '_blank'}
    ];
</script>
<slot />

