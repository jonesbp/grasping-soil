<script>
    import {onMount} from 'svelte';
    import IllustratedUnitButton from "./IllustratedUnitButton.svelte";

    let scrollY;
    let container;
    let footer;
    let banner;
    let contentWrapper;
    let bannerHeight;
    let viewportHeight;
    let title;

    let starSvg;

    $: progress = calculateProgress(scrollY);
    $: bannerStyle = calculateBannerStyle(progress);
    $: {
        if (footer) {
            if (Math.ceil(window.innerHeight + scrollY) >= document.body.offsetHeight) {
                footer.classList.add('visible');
            } else {
                footer.classList.remove('visible');
            }
        }
    }

    const calculateProgress = (scroll) => {
        if (!viewportHeight) return 0;
        const scrollRange = viewportHeight;
        return Math.min(Math.max(scroll / scrollRange, 0), 1);
    };

    const calculateBannerStyle = (progress) => {
        if (!viewportHeight || !bannerHeight) return '';

        const startPosition = (viewportHeight - bannerHeight) / 4;
        const endPosition = viewportHeight - bannerHeight;
        const currentPosition = startPosition + (progress * (endPosition - startPosition));
        const opacity = progress === 1 ? 0.2 : 1 - (progress * 0.8);

        if (progress === 1) {
            return `position: fixed; bottom: 0; left: 0; right: 0; opacity: ${opacity};`;
        }

        return `position: fixed; top: ${currentPosition}px; left: 0; right: 0; opacity: ${opacity};`;
    };

    const ensureTitleVisible = () => {
        if (!title || !banner || !contentWrapper) return;

        const titleRect = title.getBoundingClientRect();
        const viewportBottom = window.innerHeight;

        // If title is below viewport, adjust content position
        if (titleRect.top >= viewportBottom) {
            contentWrapper.style.marginTop = `${window.innerHeight / 2}px`;
        }
    };

    onMount(() => {
        viewportHeight = window.innerHeight;
        bannerHeight = banner.offsetHeight;

        // Set initial content margin to place it below the banner
        const initialBannerPosition = (viewportHeight - bannerHeight) / 4;
        contentWrapper.style.marginTop = `${initialBannerPosition + bannerHeight + 20}px`;

        progress = calculateProgress(scrollY);
        bannerStyle = calculateBannerStyle(progress);

        // Apply adjustment to make title visible if needed
        ensureTitleVisible();

        import("../assets/star.svg?raw").then(result => starSvg = result.default);

        const handleResize = () => {
            viewportHeight = window.innerHeight;
            calculateProgress(scrollY);
            calculateBannerStyle(progress);
            ensureTitleVisible();
        };

        window.addEventListener('resize', handleResize);
        return () => window.removeEventListener('resize', handleResize);
    });
</script>

<svelte:window bind:scrollY/>

<div class="container" bind:this={container}>
    <div
            class="banner"
            bind:this={banner}
            style={bannerStyle}
    >
        <div class="banner-content">
            <img src="/images/homepage-banner.jpg" width="1440" height="486" alt=""/>
        </div>
    </div>

    <!-- Content -->
    <div class="content-wrapper" bind:this={contentWrapper}>
        <div class="content">
            <h1 class="title" bind:this={title}>Grasping Soil</h1>

            <div class="intro-links">
                a <a class="syllabus-link" href="/syllabus">syllabus</a> + <a class="essays-link" href="/essays">essays</a>
            </div>
            <div class="horizontal-stars">
                {#if starSvg}
                    <span>{@html starSvg}</span>
                    <span>{@html starSvg}</span>
                    <span>{@html starSvg}</span>
                {/if}
            </div>
            <div class="abstract">
                <p class="large">
                    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc ornare, massa et vehicula pulvinar,
                    augue orci interdum lorem, sollicitudin pharetra massa mi quis ex. Sed consequat ex eget tortor
                    ultrices, non efficitur arcu fringilla. Ut accumsan erat in augue eleifend iaculis. Ut tempus justo
                    elit, quis blandit est luctus at. Sed et tellus eget sem condimentum egestas. Ut consectetur ex at
                    vulputate imperdiet. Nunc vulputate arcu massa, et interdum ante tincidunt nec. Quisque imperdiet
                    ultricies nunc eget sagittis. Mauris consequat ligula vitae nisi facilisis tempus. Fusce cursus
                    finibus varius. Praesent at dolor sit amet orci maximus ultrices.
                </p>
                <p>
                    Aliquam consectetur eu ligula quis tempus. Etiam pretium vulputate nulla et vestibulum. Suspendisse
                    potenti. Vestibulum metus enim, aliquam et massa id, consectetur accumsan eros. Interdum et
                    malesuada fames ac ante ipsum primis in faucibus. Nam enim nisl, ornare nec iaculis at, laoreet nec
                    nisi. Cras viverra at arcu a bibendum. Quisque aliquam lectus quam, sed maximus leo dictum a. Etiam
                    maximus magna vitae nisi sagittis, vitae pharetra ex mollis. Vivamus felis purus, aliquet vitae
                    ullamcorper vel, eleifend vel velit. Suspendisse rhoncus augue vel nibh vestibulum ultricies. Aenean
                    rhoncus felis id nulla tristique, nec sodales orci tincidunt. Ut sagittis nibh sed enim condimentum
                    faucibus. Nam pulvinar turpis ante, a ornare mi accumsan ut. Suspendisse ipsum est, aliquam non
                    placerat in, aliquam non odio. Etiam consequat molestie arcu, ut fermentum felis dapibus eu.
                </p>
                <p>
                    In hac habitasse platea dictumst. In massa ante, pharetra quis erat eget, blandit pretium augue.
                    Quisque rhoncus ultricies placerat. Vestibulum ac nibh ultricies, feugiat mauris eget, dictum nisi.
                    Suspendisse ac posuere tortor. Proin luctus finibus mi, id ullamcorper enim interdum vitae. Aenean
                    tempus, diam quis feugiat hendrerit, ex orci finibus lorem, non semper nisi nibh ac ex. Orci varius
                    natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Sed elementum turpis
                    magna, ut condimentum tellus lobortis eu. Morbi quis tincidunt turpis. Aenean tellus metus,
                    tristique at rutrum quis, pretium nec arcu. Suspendisse in mauris elit. Nunc lorem metus, mollis non
                    massa vel, cursus tincidunt est. Nam justo elit, consequat et metus porttitor, hendrerit porttitor
                    nisi. Fusce cursus maximus egestas. Cras mollis massa vel sollicitudin pellentesque.
                </p>
                <p>
                    Phasellus ac fringilla lectus. Fusce eleifend, nisl sed sodales aliquet, diam ipsum porttitor metus,
                    et pretium arcu quam sit amet nisi. Curabitur non malesuada ipsum. Phasellus cursus tristique
                    ligula, nec fringilla turpis tincidunt dapibus. Pellentesque fermentum, nisl vitae dignissim
                    sollicitudin, magna libero convallis dui, id fringilla lacus urna ac est. Nam consequat arcu eu
                    justo lacinia iaculis. Nunc pretium mi tellus, non pulvinar est maximus eu. Ut aliquam nisl nec urna
                    laoreet, eu sodales ligula rutrum. Curabitur lorem risus, rutrum non congue ac, convallis nec
                    libero. Mauris mollis ultrices arcu, a dapibus mi molestie vel. Nulla pulvinar tristique pharetra.
                    Etiam quis accumsan elit. Integer bibendum justo quis mattis vestibulum. Sed nec lectus lobortis,
                    placerat mi quis, condimentum nunc. Morbi dignissim congue tellus, id faucibus tortor. Nunc sed
                    libero lacus.
                </p>
            </div>
            <div class="links">
                <IllustratedUnitButton
                        unit={1}
                        title="Soil as Substrate"
                        path="/units/1-soil-as-substrate"
                />
            </div>
        </div>
    </div>

    <footer bind:this={footer}>
        <p>
            &copy; {(new Date().getFullYear())} / <a href="/colophon">Colophon</a>
        </p>
    </footer>
</div>

<style>
    h1.title {
        box-shadow: 0 10px 10px rgba(255, 255, 255, 0.9);
        background-color: white;
        color: var(--primary-text-color);
        font-family: 'Ocotillo', sans-serif;
        font-size: 5rem;
        font-weight: normal;
        line-height: 80%;
        margin: 0;
        padding: 2rem;
        position: relative;
        top: -2rem;
        right: -2rem;
        left: -2rem;
        text-align: center;
        text-transform: uppercase;
        width: calc(100% + 4rem);
    }

    .container {
        position: relative;
        min-height: 200vh;
    }

    .banner {
        width: 100%;
        transition: all 75ms;
        z-index: 1;
    }

    .banner-content {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .banner-content img {
        border-color: #96b4aa;
        border-style: solid;
        border-width: 0 0 8px 0;
        display: block;
        height: auto;
        margin: 0 auto;
        max-width: 100%;
        width: auto;
    }

    .content {
        background-color: rgba(255, 255, 255, 0.8);
        font-size: 18px;
        max-width: 800px;
        margin: 0 auto 520px auto;
        overflow: hidden;
        padding: 2rem;
        position: relative;
        width: 80%;
        z-index: 10;
    }

    .intro-links {
        color: var(--secondary-text-color);
        font-family: 'Paix', sans-serif;
        font-size: 2rem;
        margin: 0 0 3rem 0;
        text-align: center;
        text-transform: uppercase;
    }

    .intro-links a {
        color: var(--primary-text-color);
        text-decoration-color: transparent;
        transition: all .3s ease;
    }

    .intro-links a:hover {
        text-decoration-color: var(--primary-text-color);
    }

    .abstract p:first-of-type {
        margin-top: 0;
    }

    .abstract p {
        font-size: 1.1rem;
        font-weight: 300;
        line-height: 145%;
        margin: 2rem auto;
        max-width: 740px;
    }

    .abstract p.large {
        font-size: 1.4rem;
        font-weight: 400;
    }

    .horizontal-stars {
        column-gap: .5rem;
        display: flex;
        justify-content: center;
        margin-bottom: 3rem;
    }

    .horizontal-stars span {
        display: block;
        width: 1.5rem;
    }

    .horizontal-stars span :global(svg path) {
        fill: #96b4aa;
    }

    .links {
        margin-top: 4rem;
    }

    footer {
        background-color: rgba(255, 255, 255, 0.8);
        box-shadow: 0 -4px 10px rgba(255, 255, 255, 0.8);
        height: auto;
        min-height: auto;
        opacity: 0;
        padding: .5rem 2rem;
        position: fixed;
        right: 0;
        bottom: 0;
        left: 0;
        transition: .3s all ease;
        z-index: 100;
    }

    :global(footer.visible) {
        opacity: 1;
    }

    footer p {
        font-size: 1rem;
        height: auto;
        line-height: 100%;
        text-align: right;
        margin: 0;
    }

    footer a {
        color: var(--primary-text-color);
        text-decoration-color: transparent;
        transition: .1s all ease;
    }

    footer a:hover {
        text-decoration-color: var(--primary-text-color);
    }

    @media all and (max-width: 664px) {
        h1.title {
            font-size: 4.5rem;
        }
    }

    @media all and (max-width: 604px) {
        h1.title {
            font-size: 4rem;
        }

        .intro-links {
            font-size: 1.5rem;
            margin-bottom: 2rem;
        }
    }

    @media all and (max-width: 540px) {
        .content {
            width: 94%;
        }

        .abstract p.large {
            font-size: 1.2rem;
            font-weight: 400;
        }
    }

    @media all and (max-width: 454px) {
        h1.title {
            font-size: 3rem;
        }
    }

    @media all and (max-width: 366px) {
        h1.title {
            font-size: 2.5rem;
        }
    }
</style>