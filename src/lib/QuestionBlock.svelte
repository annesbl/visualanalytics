<script>
  import _ from "lodash";
  import dataset from "$data/personality_dataset.json";

  // Props → macht die Komponente wiederverwendbar für jede Frage
  export let questionText; // z. B. "How much time do you usually spend alone?"
  export let answerField; // z. B. "Time_spent_Alone"
  export let min = 1; // Slider Minimum
  export let max = 5; // Slider Maximum
  export let startValue = 3; // Default-Startwert

  let sliderValue = startValue;
  let avatars = [];

  // Avatare basierend auf aktueller Antwort updaten
  function updateAvatars(value) {
    // Personen im Datensatz mit gleicher Antwort
    const matching = dataset.filter((d) => Number(d[answerField]) === Number(value));

    // Zufällig 21 ziehen (oder weniger, falls nicht genug)
    if (matching.length >= 21) {
      avatars = _.sampleSize(matching, 21);
    } else {
      avatars = _.sampleSize(matching, matching.length);
    }
  }

  // Initial laden
  updateAvatars(sliderValue);
</script>

<div class="question-container">
  <p class="question-text">{questionText}</p>

  <input
    type="range"
    {min}
    {max}
    bind:value={sliderValue}
    on:input={() => updateAvatars(sliderValue)}
    class="slider"
  />

  <div class="avatar-grid">
    {#each avatars as person}
      <div class="avatar {person.classification}" />
    {/each}
  </div>
</div>

<style>
  .question-container {
    margin: 40px auto;
    text-align: center;
    width: 100%;
  }

  .question-text {
    font-size: 1.4rem;
    margin-bottom: 20px;
  }

  .slider {
    width: 80%;
    margin: 20px auto;
    display: block;
  }

  .avatar-grid {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 12px;
    justify-items: center;
    margin-top: 30px;
  }

  .avatar {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    transition: background 0.3s ease;
  }

  .avatar.extro {
    background: #3b82f6; /* blau */
  }

  .avatar.intro {
    background: #ef4444; /* rot */
  }
</style>
