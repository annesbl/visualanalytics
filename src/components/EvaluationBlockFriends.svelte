<script>
  import dataset from "$data/personality_dataset.json";

  export let question = "How is the size of your friends cycle?";
  export let field = "Friends_circle_size";
  export let userAnswer = 8; // später aus Quiz übergeben

  // Kategorien
  const buckets = [
    { label: "0–4", min: 0, max: 4 },
    { label: "5–9", min: 5, max: 9 },
    { label: "10–14", min: 10, max: 14 },
    { label: "15–19", min: 15, max: 19 }
  ];

  function samplePeople(min, max) {
    const filtered = dataset.filter((p) => p[field] >= min && p[field] <= max);
    const shuffled = filtered.sort(() => Math.random() - 0.5);
    return shuffled.slice(0, 5).map((p) => ({
      id: p.id,
      personality: p.Personality.toLowerCase().startsWith("extro") ? "extro" : "intro"
    }));
  }

  function bucketContains(bucket, value) {
    return value >= bucket.min && value <= bucket.max;
  }

  let grouped = buckets.map((b) => ({
    ...b,
    avatars: samplePeople(b.min, b.max)
  }));
</script>

<div class="evaluation-block">
  <h3>{question}</h3>

  <div class="circle-row">
    {#each grouped as bucket}
      <div class="circle-block">
        <div class="circle {bucketContains(bucket, userAnswer) ? 'me' : ''}">
          {#if bucketContains(bucket, userAnswer)}
            <img src="/assets/img/Me.svg" alt="You" />
          {/if}
        </div>
        <div class="avatars">
          {#each bucket.avatars as person}
            <div class="avatar-box {person.personality}">
              <img src={`/assets/img/memes/pic${Math.floor(Math.random() * 20) + 1}.png`} alt="" />
            </div>
          {/each}
        </div>
      </div>
    {/each}
  </div>
</div>

<style>
  .evaluation-block {
    margin: 4rem auto;
    text-align: center;
    width: 95%;
  }

  h3 {
    font-size: 1.5rem;
    margin-bottom: 2rem;
  }

  .circle-row {
    display: flex;
    justify-content: space-around;
    align-items: flex-start;
    gap: 1rem;
  }

  .circle-block {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
  }

  .circle {
    width: 80px;
    height: 80px;
    border: 3px solid #c9c9c9;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .circle img {
    width: 40px;
    height: 40px;
  }

  .circle.me {
    border-color: #c9a6a6;
  }

  .avatars {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .avatar-box {
    width: 60px;
    height: 60px;
    background: #f2e8df;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .avatar-box img {
    width: 40px;
    height: 40px;
  }

  .avatar-box.intro {
    background: rgba(195, 213, 255, 0.8); /* blau */
  }
  .avatar-box.extro {
    background: rgba(253, 169, 169, 0.8); /* rot */
  }
</style>
