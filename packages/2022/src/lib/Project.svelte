<script lang="ts">
  export let src: string;
  export let title: string;
  export let description: string;
  export let codeLink: string;
  export let visitLink: string;

  function handleClick(link: string) {
    window.open(link);
  }
</script>

<div class="wrapper">
  <div class="img-wrapper">
    <img {src} alt="" width="512" height="512" />
  </div>
  <div class="content">
    <h2>{title}</h2>
    <p>{description}</p>
    <div class="button-wrapper">
      <button on:click={() => handleClick(codeLink)}>Code</button>
      <button on:click={() => handleClick(visitLink)}>Visit</button>
    </div>
  </div>
</div>

<style>
  .wrapper {
    display: flex;
    gap: 128px;
  }

  :global(.isVisible) {
    filter: grayscale(1);
  }

  .img-wrapper {
    position: relative;
    transition: filter 500ms linear, transform 200ms linear;
  }

  .img-wrapper:hover {
    transform: translate(-16px, 16px);
  }
  .img-wrapper:hover::before {
    border-top-right-radius: 32px;
    border-bottom-left-radius: 0;
    left: 0px;
    bottom: 0px;
  }

  .img-wrapper::before {
    transition: all 200ms ease-in-out;
    box-sizing: border-box;
    content: "";
    position: absolute;
    width: 512px;
    height: 512px;
    border: 1px solid #ffffff;
    left: -32px;
    bottom: -32px;
    border-bottom-left-radius: 32px;
    z-index: -1;
  }

  img {
    border-top-right-radius: 32px;
    display: block;
    pointer-events: none;
    -webkit-user-drag: none;
  }

  .content {
    width: 512px;
    height: 512px;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  h2 {
    font-weight: bold;
    font-size: 40px;
    line-height: 49px;
  }

  p {
    margin-top: 32px;
    font-weight: normal;
    font-size: 18px;
    line-height: 22px;
  }

  .button-wrapper {
    margin-top: 64px;
    display: flex;
    flex-direction: row;
    gap: 64px;
  }

  button {
    padding: 16px 32px;
    border: 1px solid #ffffff;
    background: none;
    border-radius: 8px;
    font-weight: 300;
    font-size: 23px;
    line-height: 28px;
    cursor: none;
    transition: all 250ms ease-in-out;
  }

  button:hover {
    background: #ffffff;
    color: #6c19d6;
  }
</style>
