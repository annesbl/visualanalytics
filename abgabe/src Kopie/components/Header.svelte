<script>
  import wordmark from "$svg/wordmark.svg";
</script>

<header>
  <div class="wordmark">
    <img src="/assets/img/neulogo.png" alt="Title" class="title-logo" />
  </div>
</header>

<style>
  header {
    position: relative;
    z-index: 1000;
  }

  .wordmark {
  max-width: none;
  display: flex;
  justify-content: center;
  margin: 3rem auto;
}
    .title-logo {
  width: 50%;        /* nimmt 80% der Bildschirmbreite */
  max-width: 1200px; /* aber nicht breiter als 1200px */
  height: auto;
  margin-top: 5rem
}

  .wordmark a {
    border: none;
    color: var(--color-body);
  }

  .wordmark a:hover {
    background-color: transparent;
  }

  :global(.wordmark svg path) {
    fill: var(--color-body-light);
  }
</style>
