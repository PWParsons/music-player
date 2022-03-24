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

  function play() {
    isPlaying = true
  }

  function stop() {
    isPlaying = false
  }
</script>

<svelte:window
  on:keyup={(e) => e.code === 'Space' && toggleRadio()}
/>

<main class="flex flex-col h-screen justify-center items-center text-center space-y-6">
  {#await tuneIn()}
    <p>Loading...</p>
  {:then value}
    <div class="rounded-xl text-center overflow-hidden w-80 mx-auto">
      <img
        class="object-cover w-full"
        src={value.now_playing.song.art}
        alt={`${value.now_playing.song.artist} - ${value.now_playing.song.album} art`}
      >
    </div>
    
    <div>
      <audio
        on:playing={play}
        on:pause={stop}
        id="station"
        preload="all"
      >
        <source src={value.station.listen_url}>
      </audio>

      <p class="text-white">{value.now_playing.song.artist}</p>
      <p class="text-white">{value.now_playing.song.album}</p>
      <p class="text-white">{value.now_playing.song.title}</p>
    </div>
    
    <button
      on:click={toggleRadio}
      class="rounded-full shadow-2xl shadow-indigo-500 focus:outline-none ring-offset-4 ring-4 ring-indigo-500 ring-offset-slate-900 transition duration-300 hover:scale-95"
      aria-label="Play"
    >
      {#if isPlaying}
        <svg class="h-14 w-14 text-indigo-500 transition duration-300" viewBox="0 0 100 100" fill="none" xmlns="http://www.w3.org/2000/svg">
          <circle class="fill-white" opacity="0.9" cx="50" cy="50" r="50" />
          <rect class="fill-indigo-500" x="35" y="35" width="30" height="30" rx="4" />
        </svg>
      {:else}
        <svg class="h-14 w-14 text-indigo-500 transition duration-300" fill="currentColor" viewBox="0 0 84 84">
          <circle class="fill-white" opacity="0.9" cx="42" cy="42" r="42" />
          <path d="M55.5039 40.3359L37.1094 28.0729C35.7803 27.1869 34 28.1396 34 29.737V54.263C34 55.8604 35.7803 56.8131 37.1094 55.9271L55.5038 43.6641C56.6913 42.8725 56.6913 41.1275 55.5039 40.3359Z"></path>
        </svg>
      {/if}
    </button>
  {:catch error}
    <p>{error}</p>
  {/await}
</main>
