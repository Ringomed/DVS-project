<!-- 1. Update your imports to use the Inter font -->
<head>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;900&display=swap" rel="stylesheet">
</head>

<script>
    import { fade } from 'svelte/transition';
    import { createViz } from './chart.js';
    import data from './data.json';
    import { onMount } from 'svelte';

    let svgElement;
    let step = 1;  
    const maxStep = 6;

    const stories = [
        "Real Madrid is the most successful club in the 21st century and this is largely due to its squad profile.",
        "Player age is certainly one of the key parameters.",
        "But it can only be understood correctly by knowing players' playing time contribution.",
        "Real Madrid clearly uses a combination of experienced and younger players, but the team's backbone is more experienced players.",
        "But this still doesn't paint the complete picture as this team has also displayed great response to pressure, especially in the Champions League — probably due to the fact that the most used players have played many seasons together.",
        "Which is also visible from a very high mean players' stay at the club."
    ];

    function setupObservers() {
        const steps = document.querySelectorAll(".step");
        const observer = new IntersectionObserver(
            (entries) => {
                entries.forEach((entry) => {
                    if (entry.isIntersecting) {
                        step = parseInt(entry.target.dataset.index);
                    }
                });
            },
            { threshold: 0.4 }
        );
        steps.forEach((stepEl) => observer.observe(stepEl));
    }

    $: if (svgElement) {
        createViz(svgElement, data, step);
    }
    onMount(() => {
        setupObservers();
    });
</script>

<div class="container">
    <section class="intro">
        <h1>Real Madrid: A Story of Greatness</h1>
        <p>
            This journey through data reveals how Europe’s most successful club builds its success — 
            blending youth, experience, and legendary consistency.
        </p>
    </section>

    <svg bind:this={svgElement} class="chart"></svg>

    {#each stories as story, i}
        <section class="step" data-index={i+1}>
            <div class="story-box" in:fade={{ duration: 350 }}>
                <p>{story}</p>
            </div>
        </section>
    {/each}
</div>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;900&display=swap');

    :global(body) {
        background: linear-gradient(180deg, #e8eaef 0%, #f9f9fb 100%);
        font-family: 'Inter', sans-serif;
        margin: 0;
        padding: 0;
        color: #171720;
        font-size: 17px;
        min-height: 100vh;
    }

    .container {
        max-width: 900px;
        margin: auto;
        padding: 2.5em 1em 3em 1em;
        background: rgba(255,255,255,0.97);
        border-radius: 18px;
        box-shadow: 0 2px 24px rgba(44,62,80,0.09);
    }

    .intro {
        text-align: center;
        margin-bottom: 2.5em;
        padding: 2em 0 1em 0;
    }

    .intro h1 {
        color: #004996;
        font-size: 2.6em;
        font-weight: 900;
        margin-bottom: 0.5em;
        letter-spacing: -1px;
        /*background: linear-gradient(90deg,#004996,#d4af37 60%);*/
        background: #fff;
    }

    .intro p {
        font-size: 1.22em;
        color: #55556a;
        margin: 0 auto;
        max-width: 570px;
        font-weight: 400;
        line-height: 1.6;
    }

    svg.chart {
        border: 2px solid #d4af37;
		background: #fff;
        display: block;
        margin: 0 auto 3em auto;
        position: sticky;
        top: 2rem;
        border-radius: 14px;
        box-shadow: 0px 2px 16px rgba(44,62,80,0.06);
        min-height: 200px;
		z-index: 1;         
    }

    .step {
        margin: 18vh 0;
        min-height: 32vh;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .story-box {
        border: 2px solid #d4af37;
        background: #faf8fc;
        padding: 1.8em 2em;
        border-radius: 16px;
        box-shadow: 0px 2px 16px rgba(44,62,80,0.06);
        max-width: 675px;
        margin: auto;
        transition: box-shadow 0.4s cubic-bezier(.4,.1,.22,.88);
        font-weight: 500;
    }

    .story-box p {
        font-size: 1.24em;
        line-height: 1.6;
        color: #222;
        margin: 0;
    }

    /* Soft soccer vibe: subtle stripes (pitch-like) */
    :global(body)::before {
        content: "";
        position: fixed;
        top: 0; left: 0; right: 0; bottom: 0;
        background: repeating-linear-gradient(
            0deg,
            rgba(0,73,150,0.04) 0,
            rgba(0,73,150,0.04) 38px,
            transparent 38px,
            transparent 76px
        );
        pointer-events: none;
        z-index: -1;
    }

    /* Subtle fade-in for intro and chart */
    .intro, .chart, .story-box {
        animation: fadeInUp 1s both;
    }
    @keyframes fadeInUp {
        from { opacity: 0; transform: translateY(30px);}
        to { opacity: 1; transform: translateY(0);}
    }
</style>
