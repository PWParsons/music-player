<script lang="ts">
  import axios from 'axios'

  let isPlaying: boolean = false

  const tuneIn = async (): Promise<any> => {
    return axios.get('https://coderadio-admin.freecodecamp.org/api/live/nowplaying/coderadio')
      .then(response => response.data)
  }

  function toggleRadio(): void {
    const station: HTMLMediaElement = <HTMLMediaElement> document.getElementById('station')
    isPlaying ? station.pause() : station.play()
    isPlaying = !isPlaying
  }
</script>

<main>
  {#await tuneIn()}
    <p>Loading...</p>
  {:then value}
    <p>Welcome to {value.station.name}</p>

    <audio id="station" preload="all">
      <source src={value.station.listen_url}>
    </audio>
    
    <button on:click={toggleRadio}>
      {#if isPlaying} Stop {:else} Play {/if}
    </button>
  {:catch error}
    <p>Something went wrong: {error}</p>
  {/await}
</main>
