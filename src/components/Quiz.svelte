<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  // Antworten werden hier gesammelt
  let userAnswers = {
    Time_spent_Alone: "",
    Stage_fear:  "",
    Social_event_attendance: "",
    Going_outside: "",
    Drained_after_socializing: "",
    Friends_circle_size: "",
    Post_frequency: ""
  };

  function setAnswer(key, value) {
    userAnswers[key] = value;
  }

  function submitQuiz() {
    dispatch("quizComplete", userAnswers);
  }
  function getSliderPosition(value, min, max) {
  const percent = (value - min) / (max - min);
  return percent * 100; // Prozent vom Track
}

</script>

<div class="quiz-container">
  <h2>Answer these questions</h2>

  <!-- 1 -->
  <div class="question">
    <p> How many hours a day do you spend alone?</p>
    <div class="slider-wrapper">
      <input
        type="range"
        min="0"
        max="24"
        bind:value={userAnswers.Time_spent_Alone}
        class="custom-slider"
      />
      {#if userAnswers.Time_spent_Alone !== ""}
        <span
          class="slider-value"
          style="left: {getSliderPosition(userAnswers.Time_spent_Alone, 0, 24)}%;"
        >
          {userAnswers.Time_spent_Alone}
        </span>
      {/if}
    </div>
  </div>
  

  <!-- 2 -->
  <div class="question">
    <p> Do you have stage fear?</p>
    <div class="button-group">
      <button
        class:selected={userAnswers.Stage_fear === "Yes"}
        on:click={() => setAnswer("Stage_fear", "Yes")}
      >
        YES
      </button>
      <button
        class:selected={userAnswers.Stage_fear === "No"}
        on:click={() => setAnswer("Stage_fear", "No")}
      >
        NO
      </button>
    </div>
  </div>

  <!-- 3 -->
  <div class="question">
    <p> How often do you attend social events?</p>
    <div class="slider-wrapper">
      <input
        type="range"
        min="0"
        max="10"
        bind:value={userAnswers.Social_event_attendance}
        class="custom-slider"
      />
      {#if userAnswers.Social_event_attendance !== ""}
        <span
          class="slider-value"
          style="left: {getSliderPosition(userAnswers.Social_event_attendance, 0, 10)}%;"
        >
          {userAnswers.Social_event_attendance}
        </span>
      {/if}
    </div>
  </div>
  

  <!-- 4 -->
  <div class="question">
    <p> How often do you go outside in a week?</p>
    <div class="slider-wrapper">
      <input
        type="range"
        min="0"
        max="7"
        bind:value={userAnswers.Going_outside}
        class="custom-slider"
      />
      {#if userAnswers.Going_outside !== ""}
        <span
          class="slider-value"
          style="left: {getSliderPosition(userAnswers.Going_outside, 0, 7)}%;"
        >
          {userAnswers.Going_outside}
        </span>
      {/if}
    </div>
  </div>
  

  <!-- 5 -->
  <div class="question">
    <p> Do you feel drained after socializing?</p>
    <div class="button-group">
      <button
        class:selected={userAnswers.Drained_after_socializing === "Yes"}
        on:click={() => setAnswer("Drained_after_socializing", "Yes")}
      >
        YES
      </button>
      <button
        class:selected={userAnswers.Drained_after_socializing === "No"}
        on:click={() => setAnswer("Drained_after_socializing", "No")}
      >
        NO
      </button>
    </div>
  </div>

  <!-- 6 -->
  <div class="question">
    <p> How big is your circle of friends?</p>
    <div class="slider-wrapper">
      <input
        type="range"
        min="0"
        max="20"
        bind:value={userAnswers.Friends_circle_size}
        class="custom-slider"
      />
      {#if userAnswers.Friends_circle_size !== ""}
        <span
          class="slider-value"
          style="left: {getSliderPosition(userAnswers.Friends_circle_size, 0, 20)}%;"
        >
          {userAnswers.Friends_circle_size}
        </span>
      {/if}
    </div>
  </div>
  

  <!-- 7 -->
  <div class="question">
    <p> How often do you post online?</p>
    <div class="slider-wrapper">
      <input
        type="range"
        min="0"
        max="10"
        bind:value={userAnswers.Post_frequency}
        class="custom-slider"
      />
      {#if userAnswers.Post_frequency !== ""}
        <span
          class="slider-value"
          style="left: {getSliderPosition(userAnswers.Post_frequency, 0, 10)}%;"
        >
          {userAnswers.Post_frequency}
        </span>
      {/if}
    </div>
  </div>
  

  <button class="submit" on:click={submitQuiz}>See Results</button>
</div>

<style>
  .quiz-container {
    padding-top: 0.1rem;
    padding-right: 2rem;
    padding-bottom: 2rem;
    padding-left: 2rem;
    background: var(--scroll-step-background);
    border-radius: 8px;
    margin-top: 2rem;
  }

  h2 {
    font-size: 36px;
    margin-bottom: 4rem;
    color: var(--color-body);
  }
  .question span {
    display: inline-block;
    margin-top: 0rem;
    font-weight: 200;
    color: #c9a6a6;
    border: none !important;
    outline: none !important;
    background: transparent !important;
    margin-bottom: 5rem;
  }

  .button-group {
    display: flex;
    gap: 2rem;
    margin-top: 3rem;
    margin-bottom: 4rem;
    padding-left: 0rem;
    justify-content: center; 
  }

  .button-group button {
    padding: 0.75rem 1.5rem;
    border-radius: 25px;
    border: none;
    background: #c9a6a6;
    color: white;
    font-weight: standard;
    cursor: pointer;
    
  }

  .button-group button.selected {
    background: #8a6c6c;
  }

  .custom-slider {
    -webkit-appearance: none;
    width: 100%;
    height: 40px;
    background: transparent;
    outline: none;
    border: none;
    margin-bottom: 2rem;
  }

  .custom-slider::-webkit-slider-runnable-track {
    height: 40px;
    background: url('/assets/img/balken.svg') no-repeat center;
    background-size: contain;
    border: none;
    margin-top: 50px;
  }

  .custom-slider::-moz-range-track {
    height: 40px;
    background: url('/assets/img/balken.svg') no-repeat center;
    background-size: contain;
    border: none;
  }

  .custom-slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 25px;
    height: 25px;
    border-radius: 50%;
    background: #fff;
    border: 1px solid #c9a6a6;
    cursor: pointer;
    margin-top: 7.5px;
  }

  .custom-slider::-moz-range-thumb {
    width: 25px;
    height: 25px;
    border-radius: 50%;
    background: #fff;
    border: 2px solid #000;
    cursor: pointer;
  }

  .submit {
    width: 100%;
    padding: 1rem;
    background: #F2E8DF;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-size: 18px;
    color: var(--color-body);
  }
  .slider-wrapper {
  position: relative;
  width: 100%;
  margin-bottom: 4rem;
}

.slider-value {
  position: absolute;
  top: 60px; /* Höhe über Slider */
  transform: translateX(-50%);
  font-weight: 200;
  color: #c9a6a6;   /* Schriftfarbe */
  font-size: 18px; /* Schriftgröße */
  pointer-events: none;
}

</style>
