<script>
    import projects from '$lib/projects.json';
    import Project from '$lib/Project.svelte';
</script>
<svelte:head>
  <title>My Projects</title>
</svelte:head>
<h1>My Projects</h1>
    <div class="projects">
        {#each projects as p}
            <Project data={p} />
        {/each}
        <!-- <article>
            <h2>Repudiandae hic magni nulla.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Cupiditate perferendis dolores numquam dolorem asperiores dicta commodi, blanditiis repellendus atque nisi itaque sed odit voluptates ea quidem saepe tempore, pariatur accusamus. Velit dolorum distinctio aut harum eius quae esse.</p>
        </article>
        <article>
            <h2>Aspernatur, excepturi minima. Culpa.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Vitae, quidem voluptate placeat dolorum pariatur voluptatibus, a, voluptatem ratione similique rerum suscipit necessitatibus debitis doloremque culpa id veritatis quibusdam iure quam minus. Eum beatae tenetur, nemo qui totam libero.</p>
        </article>
        <article>
            <h2>Minima nam atque dignissimos.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Obcaecati cum illum pariatur modi quas ut minus aliquid sequi inventore natus reprehenderit, qui, magnam eveniet? Ipsam natus odit quisquam enim dolorum, ducimus praesentium architecto, dolore sunt mollitia accusamus! Incidunt?</p>
        </article>
        <article>
            <h2>Eum delectus debitis ad?</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Aliquam corporis ipsam nesciunt maxime praesentium. Laboriosam sunt, saepe omnis facilis quibusdam deserunt aperiam illum ratione rem eos libero porro obcaecati enim commodi culpa nam! Illum quia dolorum suscipit incidunt!</p>
        </article>
        <article>
            <h2>Similique nam cum explicabo.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Sit eius mollitia facere aut amet aliquid molestias asperiores illo eaque assumenda praesentium, neque suscipit rem similique ea doloribus iste commodi quae voluptates molestiae non recusandae magnam autem quod? Perferendis!</p>
        </article>
        <article>
            <h2>Eveniet nulla libero iusto.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Incidunt similique ullam enim necessitatibus distinctio velit quam, sint molestiae dicta temporibus error itaque blanditiis quas magnam. Enim voluptatibus quibusdam libero, tempora, omnis doloribus aut earum voluptatum esse rerum eius?</p>
        </article>
        <article>
            <h2>Repellendus exercitationem dolor omnis.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Molestiae, distinctio minima esse explicabo cupiditate molestias tempore nam pariatur deserunt aut a consequatur qui, inventore maiores fuga? Vitae sint quia animi eum laudantium suscipit sit reprehenderit cum officia omnis.</p>
        </article>
        <article>
            <h2>Est commodi debitis natus!</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Vero nihil obcaecati, mollitia adipisci suscipit nisi deserunt molestias hic minus provident dicta id esse doloremque eos! Quae quam, tempora impedit est, alias ad voluptate ullam dolorum tenetur, similique nihil!</p>
        </article>
        <article>
            <h2>Incidunt delectus distinctio itaque.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Nesciunt praesentium in ea officia reprehenderit exercitationem repellendus tenetur consequuntur nisi earum. Veritatis temporibus dolorem delectus ab ex corporis omnis fuga beatae, esse, sed dolore unde? Officiis repellendus velit odit.</p>
        </article>
        <article>
            <h2>Assumenda quam natus dolorum!</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Quia molestias id debitis, tempora magnam fugiat ex unde distinctio perferendis. Laudantium beatae in et aspernatur consequatur reprehenderit quae, eligendi aliquid quos tempore, quidem facere nobis non, explicabo inventore delectus.</p>
        </article>
        <article>
            <h2>Voluptatum autem culpa ratione.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Magnam consequuntur consequatur laboriosam aspernatur. Expedita optio molestiae vitae aliquid, impedit corrupti quod fuga repudiandae animi ipsum ratione necessitatibus id sit laudantium accusamus asperiores eos rerum laborum, quam reiciendis earum.</p>
        </article> -->
    </div>
