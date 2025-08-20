<script>
  import Hero from "./Hero.svelte";
  import Scrolly from "./helpers/Scrolly.svelte";
  import Screenshots from "./Screenshots.svelte";
  import Lenna from "./Lenna.svelte";
  import Conclusion from "./Conclusion.svelte";
  import Footer from "./Footer.svelte";
  import copy from "$data/doc.json";
  import lennaPixels from "$data/lennaPixels.json";
  import _ from "lodash";
  import { onMount } from "svelte";
  import Quiz from "./Quiz.svelte";
  let avatarInPlace = false;
  let classification = "extro"; // später aus Antworten berechnet

  onMount(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            avatarInPlace = true;
          }
        });
      },
      { threshold: 1 }
    );
    const target = document.querySelector(".row1 .gap");
    if (target) observer.observe(target);
  });

  // Quiz-Komponente
  function handleQuizComplete(e) {
    console.log("Quiz-Daten:", e.detail);
    // hier Auswertung triggern
    // Scroll zum Ergebnis-Bereich
    const resultsEl = document.getElementById("results-section");
    if (resultsEl) {
      resultsEl.scrollIntoView({ behavior: "smooth" });
    }
  }

  let mounted = false;
  let step;
  let playboyDestination;
  let scrollY = 0;
  const steps = copy.scrollProse;

  $: step, adjustStep();

  const adjustStep = () => {
    if (mounted) {
      scrollY = window.scrollY;
      if (step === undefined && scrollY > 10000) {
        step = -1;
      }
    }
  };

  onMount(() => {
    mounted = true;
    scrollY = window.scrollY;
    adjustStep();
  });

  // $: console.log({ step });
</script>

<Hero {step} />

<div class="scroll-container">
  <div class="sticky">
    <Screenshots
      mount={step === undefined || step === 5}
      visible={step >= 0 && step < 5}
      faded={step >= 2 && step < 5}
      key="memes-0"
    />
    {#each [...new Array(4).keys()] as i}
      <Screenshots
        mount={step === 1 || step === 5}
        visible={step >= 2 && step < 5}
        faded={step >= 3 && step < 5}
        picNums={[6, 7, 8, 9, 10, 11, 12]}
        key={`memes-${i + 1}`}
      />
    {/each}

    <Screenshots
      mount={step === 3 || step === 5}
      visible={step >= 4 && step < 5}
      faded={step >= 5 && step < 5}
      key="lennas"
    />

    <!-- {#if step !== undefined}
      <Lenna pixels={lennaPixels} {step} />
    {/if} -->
  </div>

  <Scrolly bind:value={step} styles={"display: flex; flex-direction: column; width: 100%;"}>
    {#each steps as { text, image }, i}
      {#if i < steps.length - 13}
        <!-- letzten 13 Steps ausblenden -->
        <div
          class="step"
          class:active={step === i || (i === 0 && step === undefined)}
          class:left={i < 4 && i % 2 === 0}
          class:right={i < 4 && i % 2 === 1}
        >
          {#if image}
            <img src={`/assets/img/story/${image}.png`} alt={image} />
          {/if}
          <p>{@html text}</p>
        </div>
      {/if}

      {#if i === 3}
        <div class="step quiz-step">
          <Quiz on:quizComplete={handleQuizComplete} />
        </div>
      {/if}
    {/each}
  </Scrolly>
</div>
<section id="results-section" class="results">
  <h2>this is you!<br />Let’s see how you are keeping up with other people</h2>

  <!-- Dein Avatar -->
  <div class="avatar-container">
    <img
      src="/assets/img/Me.svg"
      alt="You"
      class="me-avatar"
      class:in-place={avatarInPlace}
      class:intro={classification === "intro"}
      class:extro={classification === "extro"}
    />
  </div>

  <!-- Andere Avatare -->
  <div class="others-grid">
    <div class="row row1">
      <img src="/assets/img/memes/pic1.png" alt="" />
      <img src="/assets/img/memes/pic2.png" alt="" />
      <img src="/assets/img/memes/pic3.png" alt="" />
      <div class="gap" />
      <img src="/assets/img/memes/pic4.png" alt="" />
      <img src="/assets/img/memes/pic5.png" alt="" />
      <img src="/assets/img/memes/pic6.png" alt="" />
    </div>
    <div class="row row2">
      {#each [7, 8, 9, 10, 11, 12, 13] as n}
        <img src={`/assets/img/memes/pic${n}.png`} alt="" />
      {/each}
    </div>
    <div class="row row3">
      {#each [14, 15, 16, 17, 18, 19, 20] as n}
        <img src={`/assets/img/memes/pic${n}.png`} alt="" />
      {/each}
    </div>
  </div>
</section>
<Conclusion {step} />
<Footer />

<style>
  .step {
    font-size: 18px;
    width: 20em;
    margin-right: 120px;
    margin-bottom: 40vh;
    color: var(--color-body);
    background: var(--scroll-step-background);
    align-self: flex-end;
    z-index: 1000;
    opacity: 1;
  }
  .step.right {
    margin-left: auto; /* schiebt Box nach rechts */
    margin-right: 120px;
    text-align: right;
  }

  .step.left {
    margin-right: auto; /* schiebt Box nach links */
    margin-left: 120px;
    text-align: left;
  }
  .step p {
    line-height: 1.65;
    padding: 0.75rem 1.75rem;
  }

  :global(.step a) {
    text-decoration: none;
    border-bottom: 1px solid var(--base-purple-3);
  }

  :global(.step a:hover) {
    border-bottom: 2px solid var(--base-green-2);
  }

  :global(.step span) {
    font-weight: 700;
    outline: 2px solid var(--base-green-2);
    padding: 0.125rem 0.25rem;
  }

  :global(.step .bolded) {
    font-weight: 700;
    outline: none;
  }

  :global(.step .org-span) {
    background: var(--base-blue-2);
    outline: none;
    cursor: pointer;
    display: inline-block;
  }

  :global(.step .edu-span) {
    background: var(--base-orange-1);
    outline: none;
    cursor: pointer;
    display: inline-block;
  }

  :global(.step .com-span) {
    background: var(--base-orange-2);
    outline: none;
    cursor: pointer;
    display: inline-block;
  }

  :global(.step .other-span) {
    background: var(--base-tan-1);
    outline: none;
    cursor: default;
    display: inline-block;
  }

  :global(.step .org-span:hover, .step .edu-span:hover, .step .com-span:hover, .step
      .other-span:hover) {
    outline: 2px solid #282828;
  }

  .active {
    opacity: 1 !important;
  }
  .sticky {
    position: sticky;
    top: 0;
    width: 100%;
    height: 100vh;
    color: var(--color-body-light);
    font-family: var(--mono);
  }
  .scroll-container {
    width: 100%;
    position: relative;
  }

  @media only screen and (max-width: 700px) {
    .step {
      margin: 0 auto 60vh auto;
      font-size: 16px;
    }
    .step:nth-child(n + 6) {
      margin: 0 auto 90vh auto;
    }
  }
  .step:first-child {
    opacity: 1 !important;
  }

  .results {
    text-align: center;
    margin-top: 100vh; /* sorgt dafür, dass man runterscrollt */
  }

  .results h2 {
    font-size: 1.5rem;
    margin-bottom: 2rem;
  }

  .avatar-container {
    margin: 2rem 0;
    position: relative;
    display: flex;
    justify-content: center;
  }

  .me-avatar {
    width: 100px;
    height: auto;
    transition: transform 1.5s ease 2s; /* für das Runtersliden */
  }

  .others-grid {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
  }

  .row {
    display: flex;
    justify-content: center;
    gap: 1rem;
  }

  .row1 .gap {
    width: 120px; /* Lücke für dein Avatar */
  }

  .row img {
    width: 80px;
    height: 80px;
    object-fit: contain;
    filter: grayscale(100%);
    transition: filter 1s ease, transform 1.5s ease;
  }

  /* Farbe für Klassifikation */
  .intro {
    filter: grayscale(0%) sepia(1) hue-rotate(-20deg) saturate(5); /* rot */
  }
  .extro {
    filter: grayscale(0%) sepia(1) hue-rotate(180deg) saturate(5); /* blau */
  }
  .me-avatar.in-place {
    transform: translateY(100px); /* Avatar rutscht runter */
  }
</style>
