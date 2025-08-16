<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
  
    let answers = {
      Time_spent_Alone: 5,
      Stage_fear: null,
      Social_event_attendance: 5,
      Going_outside: 5,
      Drained_after_socializing: null,
      Friends_circle_size: 10,
      Post_frequency: 5
    };
  
    function handleYesNo(question, value) {
      answers[question] = value;
    }
  
    function handleSlider(question, event) {
      answers[question] = +event.target.value;
    }
  
    function submitQuiz() {
      dispatch("quizComplete", answers);
    }
  </script>
  
  <div class="quiz-container">
    <h2>Answer the questions</h2>
  
    <!-- Slider -->
    <div class="question">
      <p>1. How many hours a day do you spend alone?</p>
      <input type="range" min="0" max="10" value={answers.Time_spent_Alone} on:input={(e) => handleSlider('Time_spent_Alone', e)} />
      <span>{answers.Time_spent_Alone}</span>
    </div>
  
    <!-- Yes/No -->
    <div class="question">
      <p>2. Do you have stage fright?</p>
      <div class="yes-no-buttons">
        <button on:click={() => handleYesNo('Stage_fear', 'Yes')} class:selected={answers.Stage_fear === 'Yes'}>YES</button>
        <button on:click={() => handleYesNo('Stage_fear', 'No')} class:selected={answers.Stage_fear === 'No'}>NO</button>
      </div>
    </div>
  
    <!-- Restliche Fragen analog ... -->
  
    <button class="submit" on:click={submitQuiz}>Submit</button>
  </div>
  
  <style>
    .quiz-container {
      background: var(--base-tan-4);
      padding: 2rem;
      border-radius: 10px;
    }
    .question {
      margin-bottom: 1.5rem;
    }
    .yes-no-buttons {
      display: flex;
      gap: 1rem;
    }
    .yes-no-buttons button {
      padding: 0.5rem 1.5rem;
      border-radius: 999px;
      border: none;
      font-weight: bold;
      color: white;
      background: #b99696;
      cursor: pointer;
    }
    .yes-no-buttons button.selected {
      background: #86636b;
    }
    input[type="range"] {
      -webkit-appearance: none;
      width: 100%;
      height: 15px;
      background: url('/img/balken.svg') no-repeat;
      background-size: cover;
      border-radius: 10px;
    }
  </style>
  