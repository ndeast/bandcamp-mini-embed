<script>
  import bandcampLogoColor from "./icons/bandcamp-logotype-color.png";

  let { albumUrl } = $props();
</script>

<div class="links">
  <a href={`${albumUrl}&action=buy`}>buy</a>
  &nbsp;
  <a href={`${albumUrl}&action=share`}>share</a>
  <a class="logo" href={albumUrl}>
    <picture>
      <img src={bandcampLogoColor} alt="Bandcamp logo" />
    </picture>
  </a>
</div>

<style>
  .links {
    display: flex;
    align-items: center;
    height: auto;
    min-width: fit-content;
  }

  .links > *:first-child {
    margin-right: 8px; /* fix space between buy/share links */
  }

  .links > .logo {
    line-height: 0;
    margin-left: 8px;
  }

  .links > .logo > picture > img {
    height: 24px;
    width: auto;
  }
</style>
