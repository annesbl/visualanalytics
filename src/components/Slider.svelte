<script>
    export let min = 0;
    export let max = 10;
    export let value = 5;
  </script>
  
  <div class="slider-container">
    <input 
      type="range" 
      min={min} 
      max={max} 
      bind:value 
      class="custom-slider" 
    />
    <span>{value}</span>
  </div>
  
  <style>
    .slider-container {
      display: flex;
      flex-direction: column;
      gap: 0.4rem;
    }
  
    .custom-slider {
      -webkit-appearance: none;
      width: 100%;
      height: 50px; /* Höhe an dein SVG anpassen */
      background: url('/assets/img/balken.svg') no-repeat center;
      background-size: contain;
      outline: none;
      border: none;
    }
  
    /* Schieberegler-Knopf */
    .custom-slider::-webkit-slider-thumb {
      -webkit-appearance: none;
      width: 30px;
      height: 30px;
      border-radius: 50%;
      background: var(--base-purple);
      cursor: pointer;
    }
  
    .custom-slider::-moz-range-thumb {
      width: 30px;
      height: 30px;
      border-radius: 50%;
      background: var(--base-purple);
      cursor: pointer;
    }
  </style>
  