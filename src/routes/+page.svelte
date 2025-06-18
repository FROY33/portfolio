<script>
    let click = $state(false)  
    function handleclick() {
        click = !click    
    }  
</script>

<nav class="flex justify-between items-center bg-orange-300 text-pink-900 p-2 boreder rounded-xs">
    <a href="/"> Mi página web</a>

    
    <ul class="hidden md:flex gap-3">
        <li>Acerca de mí</li>
        <li>Portafolio</li>
        <li>Contacto</li>
    </ul>
    <div class="flex md:hidden ">
        <button onclick={handleclick}>
            =
        </button>
    </div>
</nav>

<main>
    <section>
        {#if click}
        <ul class="flex flex-col bg-orange-200 text-pink-800">
            <li>Acerca de mí</li>
            <li>Portafolio</li>
            <li>Contacto</li>
        </ul>
        {/if}
    </section>
</main>
