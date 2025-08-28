<script>
  import { onMount } from "svelte";
  import wordmark from "$svg/wordmark.svg";

  let localURL;
  let stories = [];

  const v = Date.now();
  const url = `https://pudding.cool/assets/data/stories.json?v=${v}`;

  const links = [
    { name: "about", url: "https://pudding.cool/about" },
    { name: "facebook", url: "https://facebook.com/pudding.viz/" },
    { name: "twitter", url: "https://twitter.com/puddingviz/" },
    {
      name: "instagram",
      url: "https://www.instagram.com/the.pudding"
    },
    { name: "patreon", url: "https://patreon.com/thepudding/" },
    { name: "privacy", url: "https://pudding.cool/privacy/" },
    { name: "newsletter", url: "http://eepurl.com/czym6f" },
    { name: "rss", url: "https://pudding.cool/feed/index.xml" }
  ];

  onMount(async () => {
    localURL = window.location.href;
    const response = await fetch(url);
    const data = await response.json();
    stories = data.filter((d) => d.url !== localURL).slice(0, 4);
  });
</script>

<footer>
  <section class="custom-footer">
    <!-- leer, für später -->
  </section>
</footer>

<style>
  footer {
    background-color: #f2e8df; /* deine gewünschte Hintergrundfarbe */
    color: var(--color-body);
    font-family: "Outfit", sans-serif;
    padding: 3em 1em;
    margin-top: 10em;
    text-align: center;
  }

  .custom-footer {
    min-height: 50px; /* sorgt dafür, dass der Footer sichtbar bleibt */
  }
</style>
