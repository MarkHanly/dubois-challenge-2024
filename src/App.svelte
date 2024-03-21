<script>

import { scaleLinear, scaleBand } from "d3-scale";
import { tweened } from 'svelte/motion'; // see https://connorrothschild.github.io/v4/post/svelte-scrollytelling
import { data } from "$data/Data.js";
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

let height = 600;
let width = 700;

$: innerWidth = width - margin.left - margin.right;
let innerHeight = height - margin.top - margin.bottom;

// Scrollytelling
let currentStep; 
let currentChartStep; 
let renderedData = data.filter((item) => item.nation !== "Afro-américain");
$: nations = renderedData.map(item => item.nation);  

$: {

  if(currentChartStep <1) {
    // Exclude data point for African Americans
    renderedData = data.filter((item) => item.nation !== "Afro-américain");    
    nations = renderedData.map(item => item.nation);  
    hoveredData = ""; 
  } else if (currentChartStep == 1) {
    // Exclude data point for African Americans
    renderedData = data;
    nations = renderedData.map(item => item.nation);    
  } else if (currentChartStep === 2) {
    // Exclude data point for African Americans
    hoveredData = renderedData.find(d => d.nation === "Afro-américain");
  } else if (currentChartStep === 3) {
    // Exclude data point for African Americans
    hoveredData = "";
  }
}

console.log(renderedData)

// Scales
$: yScale = scaleBand()
    .domain(nations)
    .range([0, innerHeight])
    .paddingInner(0)
    .paddingOuter(0)

$: xScale = scaleLinear()
      .domain([0, 100])
      .range([0, innerWidth]);

let hoveredData; // inialise hovered data

</script>

<!-- HTML starts here -->
<main>

    <!-- Experimenting with tweening -->
    <!-- <button on:click={setFoo}>Foo</button>
    <button on:click={setBar}>Bar</button>
    <p> Your values:
      {#each $tweenedY as x}
          {x}
      {/each}
    </p> -->

  <!-- Actual content  -->

  <section>

    <h1>Challenging racial stereotypes at the turn of the 20th century</h1> 
    <div class="content">

      <div class='wide-container'>
        <div class="chart">

            <figure>
              <img 
                alt="Carte-de-visite of Du Bois, with beard and mustache, around 39 years old"
                src='https://upload.wikimedia.org/wikipedia/commons/thumb/f/fd/W.E.B._Du_Bois_by_James_E._Purdy%2C_1907_%28cropped%29.jpg/440px-W.E.B._Du_Bois_by_James_E._Purdy%2C_1907_%28cropped%29.jpg'>
              <figcaption>W.E.B. Du Bois by James E. Purdy, 1907.</figcaption>
            </figure>
      
          </div>
        </div>

        <div class='overlay'>
          <div class='steps'>
  
            <Scrolly bind:value={currentStep}>
              {#each ['Test 1', 'Test 2', 'Test 3'] as text, i}
              <div class='step' class:active={currentStep===i}>
                <div class='step-content'>
                  <p>{text}</p>
                </div>
              </div>
              {/each}
            </Scrolly>  
            
          </div>
        </div>
  
      </div>
    </section>


  <!-- This is the chart -->
  
  <section> 

    <div class='content'>

      <div class='wide-container'>
        <div class='chart'>

          <h1>Illiteracy of African American people compared with that of other nations</h1>  
          <hr class='new'>
          <h2>Propotion d'illettrés parmi les Afro-américain comparée à celle des autres nations</h2>
          <hr class='new'>
          <h3>Done by Atlanta University.</h3>
          <div bind:clientWidth={width}>
            <svg {width} {height}>
              <g transform="translate({margin.left} {margin.top})">
                {#if currentChartStep <1}
                <Yaxis {yScale} height={innerHeight} {hoveredData}/>
                {/if}
                {#if currentChartStep >=1}
                  <Yaxis {yScale} height={innerHeight} {hoveredData}/>
                {/if}
                {#each renderedData as d, i} 
                  <rect
                    x=0
                    y={yScale(d.nation)}
                    width={xScale(d.score)}
                    height=25
                    opacity={hoveredData ? (hoveredData === d ? 1 : 0.6) : 1}
                    fill={d.nation === "Afro-américain" ? '#be0022' : '#006e41'}
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
      </div>

  
        <div class='overlay'>
          <div class='steps'>
  
            <Scrolly bind:value={currentChartStep}>
              {#each ['Step 1', 'Step 2', 'Step 3', 'Step 4'] as text, i}
              <div class='step' class:active={currentChartStep===i}>
                <div class='step-content'>
                  <p>{text}</p>
                </div>
              </div>
              {/each}
            </Scrolly>  
            
          </div>
        </div>
    </div>
  </section> 


<!-- The original chart -->
<section>
  <div class='content'>
    
    <div class='wide-container'>
      <div class='chart'>
        <figure class='fig'>
          <img 
            width=100%,  
            alt="The original W.E.B Du Bois chart titled 'Illiteracy of the American Negroes compared with that of other nations.' The chart shows illiteracy rates for 10 population groups, including African Americans of 57%, Romanians (73%) and Swedes (less than 1%)."
            src='https://tile.loc.gov/storage-services/service/pnp/ppmsca/33900/33909r.jpg'>
          <figcaption></figcaption>
        </figure>
      </div>
    </div>

    <div class='overlay'>
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
</div>
</section> 

<div class = 'reference-step'>
  current step: {currentChartStep}
</div>

</main>


<!-- CSS styling from here -->

<style>

:global(.tick text) {
    fill: #998c7e;
    user-select: none;
    transition: 
      font-size 300ms ease, 
      opacity 300ms ease,
      y 3s ease;
  }

  svg {
    background-color: #d8cec2;
  }

  rect {
    transition: 
      opacity 300ms ease,
      width 600ms ease,
      y 1s ease;
    cursor: pointer;
  }

  main {
    position: relative;
    background-color: #d8cec2;
    color: #998c7e;
    padding-left: 20%;
    padding-right: 20%;
    font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif
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
    pointer-events: none;
  }


  .steps {
    position: relative;
    z-index: 2;
    pointer-events: none;
  }


   /* Responsive containers */
  
   /* This is the main container; it moves from flex row to flex column on narrow screens */
   .content {
    display: flex;
    flex-direction: row;
  }

  /* This acts as the containing div in wide screen mode */
  .wide-container { 
	  position: sticky;
	  top: 0;
  }

  /* This is the container for visual elements */
  .chart {
    position: sticky;
    top: 0;
    z-index: 0;
    flex: 1;
  }

  /* This is the container for the text elements */
  .overlay {
    flex: 1;
    position: relative;
    opacity: 0.7;
    text-align: center;
    padding: 5px;
    z-index: 2;
    justify-content: center;
    place-items: center;
    pointer-events: none;
  }

  .steps {
    flex: 1;
    margin-right: 20px;
    position: relative;
    z-index: 2;
    pointer-events: none;
  }


  /* Rules for wider screens */
  @media (max-width: 700px) {

    main {
    position: relative;
    background-color: #d8cec2;
    color: #998c7e;
    padding-left: 2px;
    padding-right: 2px;
    font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif
  }

    .content {
      flex-direction: column;
    }

    .step-content {
    background-color: #d8cec2;
    opacity: 0.8;
    border: 1px solid black;
    padding: 0.75rem 1rem;
    border-radius: 3px;
    pointer-events: none;
  }



  }

.reference-step {
  position: fixed;
  top: 0;
  left: 0;
  padding: 1rem;
  color: red;
}

</style>