<script>
    export let userAnswers = {}; // vom Quiz
    export let evalData = {};    // Referenzdaten (Intro/Extro Verteilungen)
  
    // Funktion: holt die Auswertung für eine Antwort
    function getEvaluation(question, value) {
      if (!evalData[question]) return { intro: 0, extro: 0 };
      return evalData[question][value] || { intro: 0, extro: 0 };
    }
  
    // Aktuelle Auswertung pro Frage
    $: evaluations = Object.entries(userAnswers).map(([q, v]) => {
      return { question: q, value: v, dist: getEvaluation(q, v) };
    });
  
    // Handler: wenn User live etwas ändert
    function updateAnswer(question, value) {
      userAnswers[question] = value;
    }
  </script>
  
  <div class="evaluation">
    {#each evaluations as { question, value, dist }
    }
      <div class="eval-block">
        <!-- 📝 Platz für eigenen Text -->
        <div class="explanation">
          <p><strong>{question}</strong>: Hier könntest du eine Erklärung schreiben,
          warum diese Frage wichtig ist oder wie sie sich auf Intro/Extro auswirkt.</p>
        </div>
  
        <!-- Eingabe abhängig vom Fragetyp -->
        {#if question === "Stage_fear" || question === "Drained_after_socializing"}
          <div class="controls">
            <button
              class:selected={value === "Yes"}
              on:click={() => updateAnswer(question, "Yes")}
            >Yes</button>
            <button
              class:selected={value === "No"}
              on:click={() => updateAnswer(question, "No")}
            >No</button>
          </div>
        {:else}
          <input
            type="range"
            min="0"
            max={Object.keys(evalData[question]).length - 1}
            bind:value={userAnswers[question]}
            on:input={(e) => updateAnswer(question, e.target.value)}
          />
        {/if}
  
        <!-- Ergebnisanzeige -->
        <div class="results">
          <p>Mit deiner Antwort (<strong>{value}</strong>):</p>
          <p>{dist.intro}% Introvertiert | {dist.extro}% Extrovertiert</p>
        </div>
      </div>
    {/each}
  </div>
  
  <style>
    .evaluation {
      margin-top: 4rem;
      display: flex;
      flex-direction: column;
      gap: 4rem;
    }
  
    .eval-block {
      background: var(--scroll-step-background);
      padding: 2rem;
      border-radius: 12px;
    }
  
    .explanation {
      margin-bottom: 1.5rem;
      color: var(--color-body);
    }
  
    .controls {
      display: flex;
      gap: 1rem;
      margin-bottom: 1rem;
    }
  
    .controls button {
      padding: 0.5rem 1rem;
      border-radius: 8px;
      border: none;
      cursor: pointer;
      background: #ccc;
    }
  
    .controls button.selected {
      background: #6a4caf;
      color: white;
    }
  
    input[type="range"] {
      width: 100%;
      margin-bottom: 1rem;
    }
  
    .results {
      margin-top: 0.5rem;
      font-size: 16px;
      color: var(--color-body);
    }
  </style>
  