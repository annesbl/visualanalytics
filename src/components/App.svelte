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
  import dataset from "$data/personality_dataset.json";

  let avatarInPlace = false;
  let classification = "extro"; // wird nach Quiz gesetzt
  let showColors = false;

  // Quiz-Komponente
  function handleQuizComplete(e) {
    console.log("Quiz-Daten:", e.detail);

    // Normalisieren: Strings -> Numbers/Booleans
    const answers = {
      Time_spent_Alone: Number(e.detail.Time_spent_Alone),
      Social_event_attendance: Number(e.detail.Social_event_attendance),
      Going_outside: Number(e.detail.Going_outside),
      Friends_circle_size: Number(e.detail.Friends_circle_size),
      Post_frequency: Number(e.detail.Post_frequency),
      Stage_fear: e.detail.Stage_fear === "Yes",
      Drained_after_socializing: e.detail.Drained_after_socializing === "Yes"
    };

    classification = classifyUser(answers);
    console.log("Klassifikation:", classification);

    // Scroll zum Ergebnis
    const resultsEl = document.getElementById("results-section");
    if (resultsEl) {
      resultsEl.scrollIntoView({ behavior: "smooth" });
    }

    // Farben nach Slide aktivieren
    avatarInPlace = true;
    setTimeout(() => {
      showColors = true;
      console.log("showColors:", showColors);
    }, 1500);
  }

  let mounted = false;
  let step;
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

    // Avatar in-place triggern, wenn Gap sichtbar
    const target = document.querySelector(".row1 .gap");
    if (target) {
      const observer = new IntersectionObserver(
        (entries) => {
          entries.forEach((entry) => {
            avatarInPlace = entry.isIntersecting;
          });
        },
        { threshold: 0.3 }
      );
      observer.observe(target);
    }
  });

  // Klassifizierung
  function classifyUser(answers) {
    let bestMatch = null;
    let bestScore = Infinity;

    dataset.forEach((person) => {
      let score = 0;
      score += Math.abs(person.Time_spent_Alone - answers.Time_spent_Alone);
      score += Math.abs(person.Social_event_attendance - answers.Social_event_attendance);
      score += Math.abs(person.Going_outside - answers.Going_outside);
      score += Math.abs(person.Friends_circle_size - answers.Friends_circle_size);
      score += Math.abs(person.Post_frequency - answers.Post_frequency);

      if (person.Stage_fear !== answers.Stage_fear) score += 1;
      if (person.Drained_after_socializing !== answers.Drained_after_socializing) score += 1;

      if (score < bestScore) {
        bestScore = score;
        bestMatch = person;
      }
    });

    return bestMatch
      ? bestMatch.Personality.toLowerCase().startsWith("extro")
        ? "extro"
        : "intro"
      : "intro";
  }
</script>

<Hero {step} />

<div class="scroll-container">
  <div class="sticky">
    <!-- nur Step 0 sichtbar -->
    <Screenshots mount={step === 0} visible={step === 0} key="memes-0" />

    <!-- für die nächsten 4 Schritte genau eine Instanz sichtbar -->
    {#each [...new Array(4).keys()] as i}
      <Screenshots
        mount={step === i + 1}
        visible={step === i + 1}
        picNums={[6, 7, 8, 9, 10, 11, 12]}
        key={`memes-${i + 1}`}
      />
    {/each}

    <!-- z.B. nur Step 5 sichtbar -->
    <Screenshots mount={step === 5} visible={step === 5} key="lennas" />
  </div>

  <Scrolly bind:value={step} styles={"display: flex; flex-direction: column; width: 100%;"}>
    {#each steps as { text, image }, i}
      {#if i < steps.length - 13}
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
    <div
      class="avatar-box me-avatar"
      class:in-place={avatarInPlace}
      class:intro={classification === "intro"}
      class:extro={classification === "extro"}
    >
      <img src="/assets/img/Me.svg" alt="You" />
    </div>
  </div>

  <!-- Andere Avatare -->
  <div class="others-grid" class:colored={showColors}>
    <div class="row row1">
      <div
        class="avatar-box"
        class:intro={classification === "intro"}
        class:extro={classification === "extro"}
      >
        <img src="/assets/img/memes/pic1.png" alt="" />
      </div>
      <div
        class="avatar-box"
        class:intro={classification === "intro"}
        class:extro={classification === "extro"}
      >
        <img src="/assets/img/memes/pic2.png" alt="" />
      </div>
      <div
        class="avatar-box"
        class:intro={classification === "intro"}
        class:extro={classification === "extro"}
      >
        <img src="/assets/img/memes/pic3.png" alt="" />
      </div>
      <div class="gap" />
      <div
        class="avatar-box"
        class:intro={classification === "intro"}
        class:extro={classification === "extro"}
      >
        <img src="/assets/img/memes/pic4.png" alt="" />
      </div>
      <div
        class="avatar-box"
        class:intro={classification === "intro"}
        class:extro={classification === "extro"}
      >
        <img src="/assets/img/memes/pic5.png" alt="" />
      </div>
      <div
        class="avatar-box"
        class:intro={classification === "intro"}
        class:extro={classification === "extro"}
      >
        <img src="/assets/img/memes/pic6.png" alt="" />
      </div>
    </div>
    <div class="row row2">
      {#each [7, 8, 9, 10, 11, 12, 13] as n}
        <div
          class="avatar-box"
          class:intro={classification === "intro"}
          class:extro={classification === "extro"}
        >
          <img src={`/assets/img/memes/pic${n}.png`} alt="" />
        </div>
      {/each}
    </div>
    <div class="row row3">
      {#each [14, 15, 16, 17, 18, 19, 20] as n}
        <div
          class="avatar-box"
          class:intro={classification === "intro"}
          class:extro={classification === "extro"}
        >
          <img src={`/assets/img/memes/pic${n}.png`} alt="" />
        </div>
      {/each}
    </div>
  </div>
</section>

<Conclusion {step} />
<Footer />

<style>
  .step {
    font-size: 18px;
    width: 28em; /* breiter */
    margin-right: 120px;
    margin-bottom: 40vh;
    padding: 1.5rem; /* innen mehr Platz */
    color: var(--color-body);
    background: var(--scroll-step-background);
    border-radius: 8px; /* runde Ecken */
    align-self: flex-end;
    z-index: 1000;
    opacity: 1;
  }

  .step.right {
    margin-left: auto;
    margin-right: 120px;
    text-align: right;
  }
  .step.left {
    margin-right: auto;
    margin-left: 120px;
    text-align: left;
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

  .results {
    text-align: center;
    margin-top: 100vh;
  }
  .results h2 {
    font-size: 1.5rem;
    margin-bottom: 2rem;
  }

  .avatar-container {
    margin: 2rem 0;
    display: flex;
    justify-content: center;
  }

  .avatar-box {
    width: 90px;
    height: 90px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #f2e8df;
    transition: background-color 1s ease;
  }
  .avatar-box img {
    width: 70px;
    height: 70px;
    object-fit: contain;
  }

  .others-grid {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1.5rem;
  }

  .others-grid.colored .avatar-box.intro {
    background: rgba(195, 213, 255, 0.8); /* rot */
  }
  .others-grid.colored .avatar-box.extro {
    background: rgba(253, 169, 169, 0.8); /* blau */
  }

  .row {
    display: grid;
    grid-template-columns: repeat(7, 90px);
    gap: 1rem;
    justify-content: center;
  }
  .row1 .gap {
    width: 90px;
    height: 90px;
  }

  .me-avatar {
    transition: transform 1.5s ease 0.5s;
  }
  .me-avatar.in-place {
    transform: translateY(120px);
  }
</style>
