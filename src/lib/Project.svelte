<script lang="ts">
  import src from "../assets/one.jpg";
  export let title: string;
  export let description: string;
</script>

<div class="wrapper">
  <div class="img-wrapper">
    <img {src} alt="" width="512" height="512" />
  </div>
  <div class="content">
    <h2>{title}</h2>
    <p>{description}</p>
    <div class="button-wrapper">
      <button>Code</button><button>Visit</button>
    </div>
  </div>
</div>

<style>
  .wrapper {
    display: flex;
    gap: 128px;
  }

  .img-wrapper {
    position: relative;
  }

  .img-wrapper::before {
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
    /* cursor: pointer; */
  }
</style>
