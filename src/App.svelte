<script lang="ts">
  import axios from 'axios'

  const tuneIn = async () => {
    return axios.get('https://coderadio-admin.freecodecamp.org/api/live/nowplaying/coderadio')
      .then(response => response.data)
  }
</script>

<main>
  {#await tuneIn()}
    <p>Loading...</p>
  {:then value}
    <p>Welcome to {value.station.name}</p>
  {:catch error}
    <p>Something went wrong: {error}</p>
  {/await}
</main>
