<script lang="ts">
  import anime from "animejs";
  import { onMount } from "svelte";
  import Project from "./lib/Project.svelte";

  const lerp = (x: number, y: number, a: number) => x * (1 - a) + y * a;

  let sy = 0; // For scroll positions
  let dx = sy; // For container positions

  let cursorX = 0,
    cursorY = 0,
    cursorVX = cursorX,
    cursorVY = cursorY,
    dotVX = cursorX,
    dotVY = cursorY,
    mouseDown: boolean;

  onMount(() => {
    const slider = document.querySelector<HTMLElement>(".slider-container")!;
    const cursor = document.querySelector<HTMLElement>(".cursor")!;
    const dot = document.querySelector<HTMLElement>(".dot")!;

    slider.addEventListener("wheel", (event: WheelEvent) => {
      sy += event.deltaY;
    });

    window.addEventListener("mousedown", () => {
      mouseDown = true;
    });

    window.addEventListener("mouseup", () => {
      mouseDown = false;
    });

    document.addEventListener("mousemove", (event: MouseEvent) => {
      cursorX = event.pageX;
      cursorY = event.pageY;
    });

    window.requestAnimationFrame(render);

    function render() {
      let buttons = document.querySelectorAll<HTMLElement>("button:hover");
      if (buttons.length > 0 || mouseDown) {
        cursor.style.width = "16px";
        cursor.style.height = "16px";
        dot.style.width = "8px";
        dot.style.height = "8px";
      } else {
        cursor.style.width = "32px";
        cursor.style.height = "32px";
        dot.style.width = "4px";
        dot.style.height = "4px";
      }

      //We calculate our container position by linear interpolation method
      dx = lerp(dx, sy, 0.05);

      dx = Math.floor(dx * 100) / 100;

      slider.scrollLeft = dx;

      cursorVX = lerp(cursorVX, cursorX, 0.1);
      cursorVY = lerp(cursorVY, cursorY, 0.1);

      cursor.style.left = `${cursorVX}px`;
      cursor.style.top = `${cursorVY}px`;

      dotVX = lerp(dotVX, cursorX, 0.15);
      dotVY = lerp(dotVY, cursorY, 0.15);

      dot.style.left = `${dotVX}px`;
      dot.style.top = `${dotVY}px`;

      window.requestAnimationFrame(render);
    }

    let tl = anime.timeline({
      easing: "easeInOutExpo",
    });

    tl.add({
      targets: ".bar",
      width: "100%",
      duration: 2000,
    });

    tl.add(
      {
        targets: ".progress",
        opacity: [1, 0],
        duration: 500,
      },
      1500
    );

    tl.add({
      targets: ".column-container .column",
      translateY: ["120%", 0],
      delay: anime.stagger(150, { from: "center" }),
    });

    tl.add({
      targets: ".slider-container path",
      strokeDashoffset: [anime.setDashoffset, 0],
      easing: "easeInOutSine",
      duration: 2000,
    });

    tl.play();
  });
</script>

<div class="progress">
  <div class="bar" />
</div>
<div class="wrapper">
  <div class="column-wrapper">
    <div class="column-container">
      <div class="column" />
      <div class="column" />
      <div class="column" />
      <div class="column" />
      <div class="column" />
      <div class="column" />
      <div class="column" />
      <div class="column" />
      <div class="column" />
      <div class="column" />
      <div class="column" />
      <div class="column" />
    </div>
    <div class="slider-container">
      <div class="slider-inner">
        <div class="slide center">
          <svg
            width="277"
            height="361"
            viewBox="0 0 277 361"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              id="logo"
              d="M73 289.964L156.785 60.2405L176 7.26382C176 7.26382 111.242 -16.389 91.0894 36.2452M73 289.964C73 289.964 48.8558 359.964 144.999 359.964M73 289.964L215.849 289.965C272.161 290.964 274.999 359.964 274.999 359.964L144.999 359.964M73 289.964C73 289.964 48.8585 359.964 144.999 359.964M144.999 359.964L73.5231 359.964C62.9223 359.964 48.8573 357.813 48.8573 357.813C0.720325 348.504 -7.11435 310.16 7.82676 265.81M7.82676 265.81C40.343 176.159 58.5731 125.896 91.0894 36.2452M7.82676 265.81L91.0894 36.2452"
              stroke="white"
              stroke-width="2"
            />
          </svg>
        </div>
        <div class="slide center">
          <Project
            title="SPACEXWEB"
            description="SPACEXWEB is a web application that allows you to search for any spacex launch and see the details of the launch. The application is built with ReactJS and uses the SpaceX API to fetch the data. "
          />
        </div>
        <!-- <div class="slide center">
          <Project
            title="Asume"
            description="Lorem ipsum dolor sit amet consectetur adipisicing elit. Ad, delectus. Voluptatibus ut aspernatur architecto, vel culpa iste, illo dolor aliquid ullam modi a quam alias, corporis maiores nesciunt ea recusandae?"
          />
        </div> -->
      </div>
    </div>
  </div>
</div>
<div class="cursor" />
<div class="dot" />

<style>
  .cursor {
    box-sizing: border-box;
    position: absolute;
    top: 0;
    left: 0;
    width: 32px;
    height: 32px;
    transform: translate(-50%, -50%);
    border-radius: 50%;
    border: #fff solid 2px;
    pointer-events: none;
    transform-origin: center;
    transition: width 200ms ease-in-out, height 200ms ease-in-out,
      transform 200ms ease-in-out;
    z-index: 2;
  }
  .dot {
    content: "";
    position: absolute;
    pointer-events: none;

    width: 4px;
    height: 4px;
    transform: translate(-50%, -50%);
    border-radius: 50%;
    background: white;
    transition: width 200ms ease-in-out, height 200ms ease-in-out,
      transform 200ms ease-in-out;
  }
  .column-wrapper {
    width: 100%;
    height: 100%;
    position: relative;
  }
  .column-container {
    width: 100%;
    height: 100%;
    display: flex;
    top: 0;
    left: 0;
    position: absolute;
    z-index: -1;
  }
  .column {
    background-color: #6c19d6;
    width: 100%;
    height: 100%;
  }
  .slider-inner {
    width: 200%;
    display: flex;
  }
  .center {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .slide {
    width: 100%;
    height: 100%;
    /* scroll-snap-align: center; */
  }
  .progress {
    width: 256px;
    height: 16px;
    background-color: #dddddd;
    border-radius: 16px;
    overflow: hidden;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .bar {
    width: 0%;
    height: 100%;
    background-color: #6c19d6;
    border-radius: 16px;
  }

  .wrapper {
    box-sizing: border-box;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
    /* padding: 64px 128px; */
    padding: 64px;
    overflow: hidden;
    position: relative;
  }

  .slider-container {
    /* background-color: #6c19d6; */
    height: 100%;
    width: 100%;
    /* overflow-x: scroll; */
    display: -webkit-box;
    overflow: scroll hidden;
    /* scroll-snap-type: x proximity; */
  }
</style>
