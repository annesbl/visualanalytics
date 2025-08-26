<script>
  import { onMount } from "svelte";
  import dataset from "$data/personality_dataset.json";

  export let question = "Do you feel anxious when having to speak on stage?";
  export let field = "Stage_fear"; // Ja/Nein Feld

  let avatarsYes = [];
  let avatarsNo = [];

  function samplePeople(answer) {
    const filtered = dataset.filter((p) => {
      if (answer === "Yes") return p[field] === true || p[field] === "Yes";
      if (answer === "No") return p[field] === false || p[field] === "No";
      return false;
    });
    const shuffled = filtered.sort(() => Math.random() - 0.5);
    return shuffled.slice(0, 12); // 3x4 grid
  }

  function updateAvatars() {
    avatarsYes = samplePeople("Yes").map((p) => ({
      id: p.id,
      personality: p.Personality.toLowerCase().startsWith("extro") ? "extro" : "intro"
    }));
    avatarsNo = samplePeople("No").map((p) => ({
      id: p.id,
      personality: p.Personality.toLowerCase().startsWith("extro") ? "extro" : "intro"
    }));
  }

  onMount(() => {
    updateAvatars();
  });
</script>

<div class="evaluation-block">
  <div class="question-textblock">
    <p>{question}</p>
  </div>

  <div class="yesno-wrapper">
    <!-- NO Block -->
    <div class="block">
      <div class="grid">
        {#each avatarsNo as person}
          <div class="avatar-box {person.personality}">
            <img src={`/assets/img/memes/pic${Math.floor(Math.random() * 20) + 1}.png`} alt="" />
          </div>
        {/each}
      </div>
      <button class="option">NO</button>
    </div>

    <!-- YES Block -->
    <div class="block">
      <div class="grid">
        {#each avatarsYes as person}
          <div class="avatar-box {person.personality}">
            <img src={`/assets/img/memes/pic${Math.floor(Math.random() * 20) + 1}.png`} alt="" />
          </div>
        {/each}
      </div>
      <button class="option">YES</button>
    </div>
  </div>
</div>

<style>
  .evaluation-block {
    margin: 4rem auto;
    text-align: center;
    width: 90%;
    /* margin-bottom: 30rem;
    margin-top: 30rem;*/
  }

  h3 {
    font-size: 1.5rem;
    margin-bottom: 2rem;
  }

  .yesno-wrapper {
    display: flex;
    justify-content: space-around;
    gap: 4rem;
  }

  .block {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(4, 80px);
    gap: 1rem;
    justify-content: center;
    margin-bottom: 1rem;
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

  .option {
    padding: 0.6rem 2rem;
    border-radius: 25px;
    border: none;
    background: #c9a6a6;
    color: white;
    font-size: 1rem;
    font-weight: bold;
    cursor: default;
  }
  .question-textblock {
    background: var(--scroll-step-background);
    padding: 1rem 1.5rem;
    border-radius: 8px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
    position: relative;
    left: -5px;
    top: -70px;
    width: 520px;
    font-size: 1.2rem;
    text-align: left;
  }
</style>
