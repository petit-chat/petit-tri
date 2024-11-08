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

  function reset() {
    urls = [];
    i = 0;
  }

  async function handleFileUpload(event) {
    const file = event.target.files[0];
    if (file) {
      const text = await file.text();
      urls = text.trim().split("\n");
      i = 0;
    }
  }
</script>

<main>
  {#if urls.length === 0}
    <input type="file" onchange={handleFileUpload} />
  {:else if urls.length > i}
    <h1>{urls.length - i} left</h1>
    <iframe title="video to check" src={urls[i]}></iframe>
    <div id="sort-btn-grp" class="btn-grp">
      <button id="delete-btn" onclick={() => urls.splice(i, 1)}>Delete</button>
      <button id="keep-btn" onclick={() => i++}>Keep</button>
    </div>
  {/if}
  {#if urls.length > 0}
    <div class="btn-grp">
      <button onclick={reset}>Home</button>
      <button onclick={save}>Save ({urls.length})</button>
    </div>
  {/if}
</main>

<style>
  h1 {
    text-align: center;
    margin-bottom: 5rem;
  }

  #delete-btn {
    background-color: red;
  }

  #keep-btn {
    background-color: green;
  }

  #sort-btn-grp,
  iframe {
    margin-bottom: 5rem;
  }

  .btn-grp {
    display: flex;
    justify-content: center;
    gap: 1rem;
  }
</style>
