<script>
    import { onMount } from 'svelte';
    import { fade } from 'svelte/transition';

    let hamburgerSvg;
    let navVisible;

    const clickHandler = (e) => {
        navVisible = !navVisible;
    };

    onMount(() => {
        import("../assets/hamburger.svg?raw").then(result => hamburgerSvg = result.default);
    });
</script>

<div class="hamburger-container">
    <button on:click={clickHandler}>
        {@html hamburgerSvg}
    </button>
</div>
{#if navVisible}
    <div class="nav-overlay" transition:fade={{ duration: 200 }}>
        <ul class="top-level">
            <li>
                <a href="/syllabus">Syllabus</a>
                <ul class="unit-list">
                    <li><a href="/units/1-soil-as-substrate">Soil as Substrate</a></li>
                    <li><a href="/units/2-soil-as-archive">Soil as Archive</a></li>
                    <li><a href="/units/3-soil-as-health">Soil as Health</a></li>
                    <li><a href="/units/4-soil-as-belonging">Soil as Belonging</a></li>
                </ul>
            </li>
            <li><a href="/essays">Essays</a></li>
            <li><a href="/reading-list">Reading List</a></li>
            <li><a href="/contributors">Contributors</a></li>
        </ul>
    </div>
{/if}

<style>
.hamburger-container {
    align-items: center;
    display: flex;
    height: 48px;
    justify-content: center;
    position: fixed;
        top: 20px;
        right: 20px;
    width: 48px;
    z-index: 100;
}

button {
    align-items: center;
    background-color: white;
    border-radius: 48px;
    border-width: 0;
    cursor: pointer;
    display: flex;
    justify-content: center;
    height: 48px;
    width: 48px;
}

button svg {
    max-width: 36px;
}

.nav-overlay {
    background-color: white;
    border-color: var(--secondary-text-color);
    border-style: solid;
    border-width: 0 0 1px 1px;
    box-shadow: 0 0 2px 2px rgba(0, 0, 0, 0.1);
    padding: 80px 40px 20px 20px;
    position: fixed;
        top: 0;
        right: 0;
    z-index: 99;
}

.nav-overlay ul {
    list-style-type: none;
    margin: 0;
    padding-left: 0;
}

.nav-overlay ul.top-level {
    font-weight: 500;
}

.nav-overlay ul.top-level > li {
    margin-bottom: .5em;
}

.nav-overlay ul.top-level > li:last-of-type {
    margin-bottom: 0;
}

.nav-overlay ul.unit-list {
    list-style-type: decimal;
    font-weight: 300;
    margin-left: 24px;
}

.nav-overlay a {
    color: var(--primary-text-color);
    text-decoration-color: transparent;
    transition: .1s all ease;
}

.nav-overlay a:hover {
    color: var(--secondary-text-color);
    text-decoration-color: var(--secondary-text-color);
}
</style>