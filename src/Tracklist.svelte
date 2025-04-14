<script>
  import downChevron from "./icons/chevron-down.svg";

  let { tracks, currentTrack, play } = $props();

  let collapsed = $state(true);

  function showTracklist() {
    collapsed = false;
  }

  let artist = $derived(tracks[currentTrack].artist);
  
  // Safely decode HTML entities without using @html directive
  function decodeHtmlEntities(text) {
    const textarea = document.createElement('textarea');
    textarea.innerHTML = text;
    return textarea.value;
  }
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
        <div class="track-row">
          <div class="track-info">
            <span class="track-number">{i + 1}.</span>
            <span class="track-title">{decodeHtmlEntities(track.title)}</span>
            {#if track.artist !== artist}<span class="track-artist">– {decodeHtmlEntities(track.artist)}</span>{/if}
          </div>
          <span class="track-duration">
            {Math.floor(track.duration / 60)
              .toString()
              .padStart(2, " ")}:{Math.floor(track.duration % 60)
              .toString()
              .padStart(2, "0")}
          </span>
        </div>
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
    overflow: hidden;
    text-overflow: ellipsis;
  }
  
  .track-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    white-space: nowrap;
    width: 100%;
  }
  
  .track-info {
    flex: 1;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  
  .track-number {
    font-family: sans-serif;
    margin-right: 4px;
    margin-left: 5px;
  }
  
  .track-title {
    margin-right: 4px;
  }
  
  .track-duration {
    font-family: monospace;
    margin-left: auto;
    flex-shrink: 0;
    margin-right: 5px;
  }
  
  .tracks > .now-playing {
    font-weight: 700;
  }
  
  .tracks > .unstreamable {
    cursor: default;
    opacity: 0.5;
  }
</style>