<script>

let data = [
  {
    "nation": "Romaine",
    "score": 72.9363
  },
  {
    "nation": "Servie",
    "score": 72.2908
  },
  {
    "nation": "Russie",
    "score": 72.2908
  },
  {
    "nation": "Negroes, U.S.A.",
    "score": 56.8
  },
  {
    "nation": "Hongrie",
    "score": 55.509
  },
  {
    "nation": "Italie",
    "score": 43.8909
  },
  {
    "nation": "Autriche",
    "score": 34.8545
  },
  {
    "nation": "Ireland",
    "score": 25.1727
  },
  {
    "nation": "France",
    "score": 12.9091
  },
  {
    "nation": "Suéde",
    "score": 0.645454
  }
];

import { scaleLinear, scaleBand } from "d3-scale";
import { tweened } from 'svelte/motion'; // see https://connorrothschild.github.io/v4/post/svelte-scrollytelling
import Yaxis from "$components/Yaxis.svelte";
import Tooltip from "$components/Tooltip.svelte";
import Scrolly from "$components/Scrolly.svelte";

 // Margins
let margin={
  top: 80,
  bottom: 80,
  left: 180,
  right: 0
}

let height = 800;
let width = 700;

$: innerWidth = width - margin.left - margin.right;
let innerHeight = height - margin.top - margin.bottom;

// Scales
$: xScale = scaleLinear()
      .domain([0, 100])
      .range([0, innerWidth]);

let nations = data.map(item => item.nation);     
let yScale = scaleBand()
    .domain(nations)
    .range([0, innerHeight])
    .paddingInner(0)
    .paddingOuter(0)

let hoveredData; // inialise hovered data

// Tweened values
const tweenedY = tweened(data.map((d) => 0), {
  delay: 900,
  duration: 1000
});

const setFoo = function () {
    tweenedY.set(data.map((d) => data.indexOf(d)));
  };

  const setBar = function () {
    tweenedY.set(data.map((d) => 0));
  };


// Scrollytelling
let currentStep; 

// Screen width
let isWideScreen = true;

// Function to check screen width and set isWideScreen accordingly
function checkScreenWidth() {
  isWideScreen = window.innerWidth > 700;
}

// Event listener to check screen width on resize
window.addEventListener('resize', checkScreenWidth);

// Call checkScreenWidth on component mount
checkScreenWidth();

</script>

<!-- HTML starts here -->
<main>
  <section>

    <div class="container">
      {#if isWideScreen}
        <div class="chart">
          <!-- Chart component goes here -->
          <div class='sticky'> 
            <p>This is where the chart goes for wide screens</p>
            <img src='https://picsum.photos/id/237/200/300' width='100%'>
          </div>  
        </div>
        <div class="text">
          <!-- Text content goes here -->
          <div class='steps'>

            <Scrolly bind:value={currentStep}>
              {#each ['Step 1', 'Step 2', 'Step 3'] as text, i}
              <div class='step' class:active={currentStep===i}>
                <div class='step-content'>
                  <p>{text}</p>
                </div>
              </div>
              {/each}
            </Scrolly>  
          
          </div>
        </div>
      {:else}
          <!-- Chart component goes here -->
          <div class='sticky'> 
            <p>This is where the chart goes for narrow screens</p>
            <img src='https://picsum.photos/id/237/200/200' width='100%'>
          </div>  
          <!-- Text content goes here -->
          <div class='steps'>

            <Scrolly bind:value={currentStep}>
              {#each ['Step 1', 'Step 2', 'Step 3'] as text, i}
              <div class='step' class:active={currentStep===i}>
                <div class='step-content'>
                  <p>{text}</p>
                </div>
              </div>
              {/each}
            </Scrolly>  
          
          </div>
      {/if}
    </div>

  </section>

  <!-- Actual content  -->

  <section>

    <h1>Challenging racial stereotypes at the turn of the 20th century</h1> 
    <div class="container">
      {#if isWideScreen}
        <div class="chart">
          <!-- Chart component goes here -->
          <div class='sticky'> 
            <figure>
              <img 
            alt="Carte-de-visite of Du Bois, with beard and mustache, around 39 years old"
            src='https://upload.wikimedia.org/wikipedia/commons/thumb/f/fd/W.E.B._Du_Bois_by_James_E._Purdy%2C_1907_%28cropped%29.jpg/440px-W.E.B._Du_Bois_by_James_E._Purdy%2C_1907_%28cropped%29.jpg'>
              <figcaption>W.E.B. Du Bois by James E. Purdy, 1907.</figcaption>
            </figure>
          </div>  
        </div>
        <div class="text">
          <!-- Text content goes here -->
          <div class='steps'>

            <Scrolly bind:value={currentStep}>
              {#each ['Step 1', 'Step 2', 'Step 3'] as text, i}
              <div class='step' class:active={currentStep===i}>
                <div class='step-content'>
                  <p>{text}</p>
                </div>
              </div>
              {/each}
            </Scrolly>  
          
          </div>
        </div>
      {:else}
          <!-- Chart component goes here -->
          <div class='sticky'> 
            <figure>
              <img 
            alt="Carte-de-visite of Du Bois, with beard and mustache, around 39 years old"
            src='https://upload.wikimedia.org/wikipedia/commons/thumb/f/fd/W.E.B._Du_Bois_by_James_E._Purdy%2C_1907_%28cropped%29.jpg/440px-W.E.B._Du_Bois_by_James_E._Purdy%2C_1907_%28cropped%29.jpg'>
            <figcaption>W.E.B. Du Bois by James E. Purdy, 1907.</figcaption>
            </figure>
          </div>  
          <!-- Text content goes here -->
          <div class='steps'>

            <Scrolly bind:value={currentStep}>
              {#each ['Step 1', 'Step 2', 'Step 3'] as text, i}
              <div class='step' class:active={currentStep===i}>
                <div class='step-content'>
                  <p>{text}</p>
                </div>
              </div>
              {/each}
            </Scrolly>  
          
          </div>
      {/if}
    </div>


  <!-- This is the chart -->
  <section> 
  <div class='sticky'>

    
    <!-- <button on:click={setFoo}>Foo</button>
    <button on:click={setBar}>Bar</button>
    <p> Your values:
      {#each $tweenedY as x}
          {x}
      {/each}
    </p> -->

  <h1>Illiteracy of African American people compared with that of other nations</h1>  
  <hr class='new'>
  <h2>Propotion d'illettrés parmi les Nègres Americans comparée à celle des autres nations</h2>
  <hr class='new'>
  <h3>Done by Atlanta University.</h3>
  <div class="chart-container" bind:clientWidth={width}>
    <svg {width} {height}>
      <g transform="translate({margin.left} {margin.top})">
        <Yaxis {yScale} height={innerHeight} {hoveredData}/>
        {#each data as d, i} 
          <rect
            x=0
            y={yScale(d.nation)}
            width={xScale(d.score)}
            height=35
            opacity={hoveredData ? (hoveredData === d ? 1 : 0.6) : 1}
            fill={d.nation === "Negroes, U.S.A." ? '#be0022' : '#006e41'}
            on:mouseover={() => {
              hoveredData = d;
            }}
            on:focus={() => {
              hoveredData = d;
            }}
            on:mouseleave={() => {
              hoveredData = "";
            }}
            />
        {/each}
      </g>

    </svg>
    {#if hoveredData}
    <Tooltip data={hoveredData} {xScale} {yScale} {margin} />
  {/if}
  </div>

  </div>

<div class='steps'>

  <Scrolly bind:value={currentStep}>
    {#each ['a', 'b', 'c'] as text, i}
    <div class='step' class:active={currentStep===i}>
      <div class='step-content'>
        <p>{text}</p>
      </div>
    </div>
    {/each}
  </Scrolly>  

</div>
</section>


<section>
  <div class='sticky'>
    <figure>
      <img 
    alt="The original W.E.B Du Bois chart titled 'Illiteracy of the American Negroes compared with that of other nations.' The chart shows illiteracy rates for 10 population groups, including African Americans of 57%, Romanians (73%) and Swedes (less than 1%)."
    src='https://tile.loc.gov/storage-services/service/pnp/ppmsca/33900/33909r.jpg'>
      <figcaption></figcaption>
    </figure>
  </div>

  <div class='steps'>
    <Scrolly bind:value={currentStep}>
      {#each ['For comparison, here is the original chart created by W.E.B Du Bois and presented at the Paris Exposition Universelle in 1900'] as text, i}
      <div class='step' class:active={currentStep===i}>
        <div class='step-content'>
          <p>{text}</p>
        </div>
      </div>
      {/each}
    </Scrolly>  
  </div>

</section>


<!-- <section>
  <div class='sticky'>
    
  </div>

  <div class='steps'>
    <Scrolly bind:value={currentStep}>
      {#each ['a', 'b', 'c'] as text, i}
      <div class='step' class:active={currentStep===i}>
        <div class='step-content'>
          <p>{text}</p>
        </div>
      </div>
      {/each}
    </Scrolly>  
  </div>

</section> -->

</main>



<style>

:global(.tick text) {
    fill: #998c7e;
    user-select: none;
    transition: 
      font-size 300ms ease, 
      opacity 300ms ease;
  }

  svg {
    background-color: #d8cec2;
  }

  rect {
    transition: 
      opacity 300ms ease,
      width 2s ease;
    cursor: pointer;
  }

  h1 {
    padding-top: 40px;
    padding-bottom: 10px;
    font-size: 1.3em;
    font-weight: 600;
    text-align: center;
  }

  h2 {
    padding-top: 10px;
    padding-bottom: 10px;
    font-size: 1em;
    text-align: center;
  }

  h3 {
    padding-top: 10px;
    font-size: 0.8em;
    text-align: center;
  }

  hr.new {
    border: 0.2px solid #998c7e;
    width:20%;
    opacity: 0.3;
    margin: 0 auto;
  }

  main {
    position: relative;
    background-color: #d8cec2;
    color: #998c7e;
    font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif
  }

  .step {
    height: 90vh;
    opacity: 0.3;
    color:black;
    transition: opacity 600ms ease;
    display: flex;
    justify-content: center;
    place-items: center;
  }

  .step.active {
    opacity: 1;
  }

  .step-content {
    background-color: #d8cec2;
    opacity: 0.8;
    border: 1px solid black;
    padding: 0.75rem 1rem;
    border-radius: 3px;
  }


  .steps {
    position: relative;
    z-index: 2;
    pointer-events: none;
  }

  .sticky {
    position: sticky;
    top: 0;
    z-index: 1;
  }

  .narrative h1 {
    padding-top: 40px;
    padding-bottom: 40px;
    font-size: 2em;
    font-weight: 600;
    text-align: center;
  }


   /* Responsive containers */
   .container {
    display: flex;
    flex-direction: column;
  }

  .chart {
    width: 100%;
    max-width: 100%;
    margin-bottom: 20px;
  }
  .steps {
    flex: 1;
    margin-right: 20px;
    position: relative;
    z-index: 2;
    pointer-events: none;
  }

  /* .text {
      flex: 1;
      margin-right: 20px;
      position: relative;
      z-index: 2;
      background-color: pink;
      pointer-events: none;
    }  */

  @media screen and (min-width: 701px) {
    .container {
      flex-direction: row;
    }

    .text {
      flex: 1;
      margin-right: 20px;
      background-color: whitesmoke;
    }

    .chart {
      flex: 1;
      order: -1; /* Move chart to the left */
    }
  }

  .sticky figure {
      width: 100%;
      text-align: center;
      margin: auto;
  }

  .buffer {
    height: 1000px;
  }

</style>
