<script>
  export let store

  let uploader

  const upload = () => store(uploader.files[0])
</script>

<div>
  <div>
    <input bind:this={uploader} type="file" />
    <button on:click={upload}>Process</button>
  </div>
</div>
