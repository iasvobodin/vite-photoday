<script>
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";
  import Scrollbar from "smooth-scrollbar";
  import { onMount } from "svelte";
  // Using Locomotive Scroll from Locomotive https://github.com/locomotivemtl/locomotive-scroll
  // gsap.core.globals('ScrollTrigger', ScrollTrigger);
  // onMount(async () => {
  // 	const locomotiveModule = await import('locomotive-scroll');
  // 	// scroll = new locomotiveModule.default({
  // 	locoScroll = new locomotiveModule.default({
  // 		el: document.querySelector('.smooth-scroll'),
  // 		smooth: true
  // 	});
  // 	locoScroll.on('scroll', ScrollTrigger.update);
  // 	// each time Locomotive Scroll updates, tell ScrollTrigger to update too (sync positioning)

  // 	// tell ScrollTrigger to use these proxy methods for the ".smooth-scroll" element since Locomotive Scroll is hijacking things
  // 	ScrollTrigger.scrollerProxy('.smooth-scroll', {
  // 		scrollTop(value) {
  // 			return arguments.length
  // 				? locoScroll.scrollTo(value, 0, 0)
  // 				: locoScroll.scroll.instance.scroll.y;
  // 		}, // we don't have to define a scrollLeft because we're only scrolling vertically.
  // 		getBoundingClientRect() {
  // 			return { top: 0, left: 0, width: window.innerWidth, height: window.innerHeight };
  // 		},
  // 		// LocomotiveScroll handles things completely differently on mobile devices - it doesn't even transform the container at all! So to get the correct behavior and avoid jitters, we should pin things with position: fixed on mobile. We sense it by checking to see if there's a transform applied to the container (the LocomotiveScroll-controlled element).
  // 		pinType: document.querySelector('.smooth-scroll').style.transform ? 'transform' : 'fixed'
  // 	});

  // 	// --- RED PANEL ---
  // 	gsap.from('.line-1', {
  // 		scrollTrigger: {
  // 			trigger: '.line-1',
  // 			scroller: '.smooth-scroll',
  // 			scrub: true,
  // 			start: 'top bottom',
  // 			end: 'top top'
  // 		},
  // 		scaleX: 0,
  // 		transformOrigin: 'left center',
  // 		ease: 'none'
  // 	});

  // 	// --- ORANGE PANEL ---
  // 	gsap.from('.line-2', {
  // 		scrollTrigger: {
  // 			trigger: '.orange',
  // 			scroller: '.smooth-scroll',
  // 			scrub: true,
  // 			pin: true,
  // 			start: 'top top',
  // 			end: '+=100%'
  // 		},
  // 		scaleX: 0,
  // 		transformOrigin: 'left center',
  // 		ease: 'none'
  // 	});

  // 	// --- PURPLE/GREEN PANEL ---
  // 	var tl = gsap.timeline({
  // 		scrollTrigger: {
  // 			trigger: '.purple',
  // 			scroller: '.smooth-scroll',
  // 			scrub: true,
  // 			pin: true,
  // 			start: 'top top',
  // 			end: '+=100%'
  // 		}
  // 	});

  // 	tl.from('.purple p', { scale: 0.3, rotation: 45, autoAlpha: 0, ease: 'power2' })
  // 		.from('.line-3', { scaleX: 0, transformOrigin: 'left center', ease: 'none' }, 0)
  // 		.to('.purple', { backgroundColor: '#28a92b' }, 0);

  // 	// each time the window updates, we should refresh ScrollTrigger and then update LocomotiveScroll.
  // 	ScrollTrigger.addEventListener('refresh', () => locoScroll.update());

  // 	// after everything is set up, refresh() ScrollTrigger and update LocomotiveScroll because padding may have been added for pinning, etc.
  // 	ScrollTrigger.refresh();
  // });
  onMount(() => {
    // const { ScrollTrigger } = await import('gsap/ScrollTrigger');
    // const  = st.default;
    gsap.registerPlugin(ScrollTrigger);

    const scroller = document.querySelector(".scroller");

    const bodyScrollBar = Scrollbar.init(scroller, {
      damping: 0.1,
      delegateTo: document,
      alwaysShowTracks: true,
    });

    ScrollTrigger.scrollerProxy(".scroller", {
      scrollTop(value) {
        if (arguments.length) {
          bodyScrollBar.scrollTop = value;
        }
        return bodyScrollBar.scrollTop;
      },
    });

    bodyScrollBar.addListener(ScrollTrigger.update);

    ScrollTrigger.defaults({ scroller: scroller });

    // The actual animations and ScrollTriggers
    gsap.to("section.grey .text", {
      rotation: 360,
      scrollTrigger: {
        trigger: "section.grey",
        start: "top top",
        end: () => "+=" + innerHeight,
        pin: true,
        scrub: true,
        markers: true,
      },
    });

    gsap.from("section.red .text", {
      x: -500,
      opacity: 0,
      scrollTrigger: {
        trigger: "section.red",
        start: "top 50%",
        toggleActions: "play none none reset",
        // markers:true
      },
    });
  });
</script>

<div class="scroller">
  <section class="green">
    <div class="heading">ScrollTrigger & Smooth-Scrollbar</div>
    <!-- <div class="text">Section 1</div> -->
  </section>

  <section class="grey">
    <div class="text">Section 2</div>
  </section>

  <section class="red">
    <div class="text">Section 3</div>
  </section>
</div>

<svelte:head>
  <link
    href="//fonts.googleapis.com/css?family=Signika+Negative:300,400,600"
    rel="stylesheet"
    type="text/css"
  />
  <meta name="viewport" content="user-scalable=no, width=device-width" />
</svelte:head>

<style>
  @import url("https://fonts.googleapis.com/css?family=Signika+Negative:300,400&display=swap");

  :global(body) {
    font-family: "Signika Negative", sans-serif;

    margin: 0;
    padding: 0;

    height: 100%;
    width: 100%;

    overflow: hidden;
  }

  .scroller {
    height: 100vh;
  }

  section {
    height: 100vh;
    width: 100%;
    display: flex;
    flex-direction: column;
  }

  section .heading {
    position: relative;
    margin: auto;
    color: darkgoldenrod;
    font-size: 2.5em;
  }

  section .text {
    position: relative;
    margin: auto;
    color: white;
    font-size: 2em;
  }

  section.green {
    background: lightblue;
  }

  section.grey {
    background: grey;
  }

  section.red {
    background: steelblue;
  }
</style>
