<script>
  import { onMount } from "svelte";
  import dataset from "$data/personality_dataset.json";

  export let question = "How many hours of the day do you spend alone?";
  export let field = "Time_spent_Alone"; // Feld im Datensatz
  export let min = 0;
  export let max = 24;
  export let userAnswer = null; // vom Quiz übergeben

  let selectedValue = 5; // default
  let avatars = [];

  function getSliderPosition(value, min, max) {
    const percent = ((value - min) / (max - min)) * 100;
    return percent;
  }

  // Funktion: 27 zufällige Personen mit dieser Antwort holen
  function samplePeople(answer) {
    const filtered = dataset.filter((p) => p[field] == answer);
    const shuffled = filtered.sort(() => Math.random() - 0.5);
    return shuffled.slice(0, 21); // 3x7 grid
  }

  function updateAvatars() {
    const people = samplePeople(selectedValue);

    // echte Personen abbilden
    let mapped = people.map((p) => ({
      id: p.id,
      personality: p.Personality.toLowerCase().startsWith("extro") ? "extro" : "intro"
    }));

    // fehlende Plätze mit neutralen Avataren auffüllen
    while (mapped.length < 21) {
      mapped.push({ id: `empty-${mapped.length}`, personality: "none" });
    }

    avatars = mapped;
  }

  $: selectedValue, updateAvatars();

  onMount(() => {
    updateAvatars();
  });
</script>

<div class="evaluation-block">
  <!--<h3>{question}</h3>-->

  <!-- Avatar Grid -->
  <div class="grid">
    {#each avatars as person}
      <div class="avatar-box {person.personality}">
        <img src={`/assets/img/memes/pic${Math.floor(Math.random() * 20) + 1}.png`} alt="" />
      </div>
    {/each}
  </div>

  <!-- Slider -->
  <div class="slider-wrapper">
    <input type="range" {min} {max} bind:value={selectedValue} class="custom-slider" />

    <!-- Dein eigener Avatar bleibt fix an deiner Antwort -->
    {#if userAnswer !== null}
      <div class="me-avatar" style="left: {getSliderPosition(userAnswer, min, max)}%;">
        <img src="/assets/img/Me.svg" alt="Me" />
      </div>
    {/if}

    <!-- Aktuelle Auswahl-Anzeige -->
    {#if selectedValue !== ""}
      <span class="slider-value" style="left: {getSliderPosition(selectedValue, min, max)}%;">
        {selectedValue}
      </span>
    {/if}
  </div>
</div>

<style>
  .evaluation-block {
    margin: 30rem auto;
    text-align: center;
    width: 80%;
  }

  h3 {
    font-size: 1.5rem;
    margin-bottom: 2rem;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(7, 80px);
    gap: 1rem;
    justify-content: center;
    margin-bottom: 5rem;
  }

  .avatar-box {
    width: 80px;
    height: 80px;
    background: #f2e8df;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .avatar-box img {
    width: 60px;
    height: 60px;
  }

  .avatar-box.intro {
    background: rgba(195, 213, 255, 0.8); /* blau */
  }
  .avatar-box.extro {
    background: rgba(253, 169, 169, 0.8); /* rot */
  }

  .slider-wrapper {
    position: relative; /* wichtig, damit Avatar absolute positioniert werden kann */
    width: 80%; /* hier statt 100% enger machen */
    max-width: 630px; /* feste Max-Breite falls gewünscht */
    margin: 0 auto 4rem auto;
  }

  .custom-slider {
    -webkit-appearance: none;
    width: 100%;
    height: 40px;
    background: transparent;
    border: none;
    position: relative;
  }

  .custom-slider::-webkit-slider-runnable-track {
    height: 40px;
    border-radius: 20px;
    background: linear-gradient(90deg, #e6a195, #6b364c);
    /* WICHTIG: volle Breite nutzen */
    background-size: 100% 100%;
  }

  .custom-slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 25px;
    height: 25px;
    border-radius: 50%;
    background: #fff;
    border: 2px solid #c9a6a6;
    cursor: pointer;
    margin-top: 7.5px;

    /* sorgt dafür, dass der Thumb nicht mehr "übersteht" */
    position: relative;
    z-index: 2;
  }

  /* Firefox */
  .custom-slider::-moz-range-track {
    height: 40px;
    border-radius: 20px;
    background: linear-gradient(90deg, #e6a195, #6b364c);
  }

  .custom-slider::-moz-range-thumb {
    width: 25px;
    height: 25px;
    border-radius: 50%;
    background: #fff;
    border: 2px solid #c9a6a6;
    cursor: pointer;
  }
  .slider-value {
    position: absolute;
    top: 45px;
    transform: translateX(-50%);
    font-weight: 200;
    color: #c9a6a6;
    font-size: 18px;
  }
  .me-avatar {
    position: absolute;
    top: 5px; /* über dem Slider platzieren */
    transform: translateX(-50%);
    background: #e9dfd7; /* deine Wunschfarbe */
    border-radius: 10%;
    width: 30px;
    height: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10; /* Avatar über Slider sichtbar */
    pointer-events: none; /* nicht klickbar */
  }

  .me-avatar img {
    width: 28px;
    height: 28px;
  }
  .avatar-box.none {
    background: #f2e8df; /* neutraler Hintergrund, wie am Anfang */
  }
</style>
