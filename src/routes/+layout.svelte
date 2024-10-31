<nav>
    {#each pages as p}
        <a href={p.url} 
        target={p.url.startsWith("http") ? "_blank" : "_self"} 
        class:current={'.' + $page.route.id === p.url}>
            {p.title}
        </a>
        
    {/each}
</nav>


<label class="color-scheme">
    Theme:
    <select id='color-scheme-select' bind:value= {colorScheme}>
        <option value="light dark">Automatic</option>
        <option value="light">Light</option>
        <option value="dark">Dark</option>
    </select>
</label>


<script>
    import { page } from '$app/stores';
    import '../style.css';
    let localStorage = globalThis.localStorage ?? {};
    let colorScheme = localStorage.colorScheme || 'light dark';
    
    let root = globalThis?.document?.documentElement;
    $: root?.style.setProperty('color-scheme', colorScheme);
    $: localStorage.colorScheme = colorScheme;

    let pages = [
        {url: './', title: 'Home' },
        {url: './projects', title: 'Projects' },
        {url: './contact', title:'Contact' },
        {url: './resume', title: 'Resume'},
        {url: 'https://github.com/nshor47', title:'Github', target: '_blank'}
    ];
</script>

<style>
    .color-scheme {
    position: absolute;
    top: 1rem;
    right: 1rem;
    font-size: 80%;
    font-family: inherit;
    }

    :root[data-theme='dark'] {
    --color-bg: #121212;
    --color-header-bg: #1e1e1e;
    --color-nav-bg: #333;
    --color-nav-text: #fff;
    --color-footer-bg: #1e1e1e;
    --color-footer-text: #bbb;
    --border-color: oklch(50% 10% 200 / 40%);
    }
</style>
<slot />

