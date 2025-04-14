<script>
  import downChevron from "./icons/chevron-down.svg";

  let { tracks, currentTrack, play } = $props();

  let collapsed = $state(true);

  function showTracklist() {
    collapsed = false;
  }

  let artist = $derived(tracks[currentTrack].artist);
</script>

<div class="tracklist">
  <div class="tracks">
    {#each tracks as track, i}
      <button
        disabled={!tracks[i].track_streaming}
        class:now-playing={i === currentTrack}
        class:unstreamable={!tracks[i].track_streaming}
        onclick={() => tracks[i].track_streaming && play(i)}
        onkeydown={(e) => {
          if (
            tracks[i].track_streaming &&
            (e.key === " " || e.key === "Enter")
          ) {
            e.preventDefault();
            play(i);
          }
        }}
      >
        <span>
          {Math.floor(track.duration / 60)
            .toString()
            .padStart(2, " ")}:{Math.floor(track.duration % 60)
            .toString()
            .padStart(2, "0")}
        </span>
        {@html "  " + track.title}
        {#if track.artist !== artist}– {track.artist}{/if}
      </button>
    {/each}
  </div>
</div>

<style>
  .tracklist {
    box-sizing: border-box;
    overflow-y: scroll;
    max-height: 80px;
    transition: max-height 0.2s;
  }

  .tracks {
    margin: 0;
    padding: 8px 0;
  }
  .tracks > * {
    display: block;
    width: 100%;
    text-align: left;
    font-family: sans-serif;
    background: none;
    font-size: 12px;
    cursor: pointer;
    padding: 10px 8px;
    border: 0;
    border-bottom: 1px solid #bbb;
    white-space: pre;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .tracks > .now-playing {
    font-weight: 700;
  }
  .tracks > .unstreamable {
    cursor: default;
    opacity: 0.5;
  }
  .tracks span {
    font-family: monospace;
  }
</style>
