<script>
  let urls = $state([]);
  let i = $state(0);

  function save() {
    const blob = new Blob([urls.join("\n")], { type: "text/plain" });
    const link = document.createElement("a");
    link.href = URL.createObjectURL(blob);
    link.download = "urls.txt";
    link.click();
  }

  function reject() {
    if (confirm("are you sure you want to reject?")) {
      urls.splice(i, 1);
    }
  }

  function reset() {
    if (confirm("are you sure you want to reset?")) {
      urls = [];
      i = 0;
    }
  }

  async function handleFileUpload(event) {
    const file = event.target.files[0];
    if (file) {
      const text = await file.text();
      urls = text.trim().split("\n");
    }
  }
</script>

<main>
  {#if urls.length === 0}
    <h1>watch and triage video urls</h1>
    <input
      type="file"
      onchange={handleFileUpload}
      aria-label="upload url list"
    />
  {:else if urls.length > i}
    <h1>{urls.length - i} left</h1>
    {#key urls[i]}
      <!-- svelte-ignore a11y_media_has_caption -->
      <video autoplay controls width="340">
        <source src={urls[i]} />
        video not supported on your browser
      </video>
    {/key}
    <div id="sort-btn-grp" class="btn-grp">
      <button onclick={reject}>Reject</button>
      <button onclick={() => i++}>Approve</button>
    </div>
  {:else if urls.length === i}
    <h1>done</h1>
  {/if}
  {#if urls.length > 0}
    <div class="btn-grp">
      <button onclick={reset}>Reset</button>
      <button onclick={save}>Save ({urls.length} approved)</button>
    </div>
  {/if}
</main>

<style>
  .btn-grp {
    display: flex;
    justify-content: center;
    gap: 1rem;
    margin-top: 2rem;
  }
</style>
