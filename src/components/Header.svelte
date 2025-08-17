<script>
  import wordmark from "$svg/wordmark.svg";
</script>

<header>
  <div class="wordmark">
    <img src="/assets/img/title.svg" alt="Title" class="title-logo" />
  </div>
</header>

<style>
  header {
    position: relative;
    z-index: 1000;
  }

  .wordmark {
    max-width: 10em;
    /*margin: 0 auto;*/
    padding: 1em 0 0 0;display: flex;
    justify-content: center;  /* zentriert horizontal */
    align-items: center;      /* zentriert vertikal */
    margin: 2rem auto;
  }
    .title-logo {
    max-width: 800px;   /* Titelgröße */
    height: auto;
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
