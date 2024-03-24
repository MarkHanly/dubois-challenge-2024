<script>

import { scaleLinear, scaleBand } from "d3-scale";
import { data } from "$data/Data.js";
import Narrative from "$components/Narrative.svelte";
import Yaxis from "$components/Yaxis.svelte";
import Tooltip from "$components/Tooltip.svelte";
import Scrolly from "$components/Scrolly.svelte";
import Denouement from "./components/Denouement.svelte";
import Footer from "./components/Footer.svelte";
import Quote from "./components/Quote.svelte";
import Spacer from "./components/Spacer.svelte";

// Narrative

let scene1 = 
['Here is a photograph of Du Bois taken by James Purdy in 1907, when Du Bois was 39 years old.', 
'The same age that I am now.', 
"And I'd be the first to admit that as well as being a scholar and a gentleman he is a handsome divil."];

let scene3 = 
['This chart compares the degree of illiteracy among several populations.', 
'There is a wide range of illiteracy levels evident, from almost 73% illiteracy among Romanians (Romaine).', 
'To less than 1% for Swedes (Suéde). Tap or hover on the bars to explore the data.', 
'Considering that slavery had ended with the adoption of the 13th amendment in 1865, thirty-five years previously, where do you think the bar lies for African American people on this chart?',
"Seriously, take a moment to stop and think about it. That's only about the same span of time between 2024 and when The Pixies released Doolittle. I saw them play last year!",
'In the middle—56.8%! Just one generation out from the end of slavery, almost half of all African Americans were literate, equalling or exceeding literacy rates among several European countries.',
'With this relatively simple data visualisation, Du Bois was challenging the pre-conceived stereotypes about African Americans held by many of the Europeans visiting the Paris exposition.'];

let scene4 = 
[
  'Here is the original chart, produced by W.E.B. Du Bois in 1900.',
  'Working at the turn of the century, a time when the field of modern statistics was just starting to gain momentum, Du Bois was already pioneering techniques which are now data visualisation standards.',
  'Notable is the use of a stand-out colour—here red—to emphasise one data point or population and connect that to a broader narrative, a ubiquitous trick in modern media. '
];


// Margins
let margin={
  top: 40,
  bottom: 0,
  left: 150,
  right: 0
}

let height = 640;
let width = 510;

$: innerWidth = width - margin.left - margin.right;
let innerHeight = height - margin.top - margin.bottom;

// Scrollytelling
let currentStep; 
let currentChartStep; 
$: renderedData = data.filter((item) => item.nation !== "Afro-Américain");
$: nations = renderedData.map(item => item.nation);  
$: hoveredData = "";

$: {

  if(currentChartStep === 0) {
    // Exclude data point for African Americans
    renderedData = data.filter((item) => item.nation !== "Afro-Américain");    
    nations = renderedData.map(item => item.nation);  
    hoveredData = "";  
  } else if (currentChartStep === 1) {
    // Hover on Romaine
    renderedData = data.filter((item) => item.nation !== "Afro-Américain");    
    nations = renderedData.map(item => item.nation);  
    hoveredData = renderedData.find(d => d.nation === "Romaine");
  } else if (currentChartStep === 2) {
    // Hover on Suéde
    renderedData = data.filter((item) => item.nation !== "Afro-Américain");    
    nations = renderedData.map(item => item.nation);  
    hoveredData = renderedData.find(d => d.nation === "Suéde");
  } else if (currentChartStep === 3) {
    // Hover on Romaine
    renderedData = data.filter((item) => item.nation !== "Afro-Américain");    
    nations = renderedData.map(item => item.nation);  
    hoveredData = "";
  } else if (currentChartStep === 4) {
    // Hover on Romaine
    renderedData = data.filter((item) => item.nation !== "Afro-Américain");    
    nations = renderedData.map(item => item.nation);  
    hoveredData = "";
  } else if (currentChartStep === 5) {
    // Include data point for African Americans
    renderedData = data;
    nations = renderedData.map(item => item.nation);  
    hoveredData = renderedData.find(d => d.nation === "Afro-Américain"); 
  } else if (currentChartStep === 6) {
    // Include data point for African Americans
    renderedData = data;
    nations = renderedData.map(item => item.nation);  
    hoveredData = ""; 
  }
}

// Scales
$: yScale = scaleBand()
    .domain(nations)
    .range([0, innerHeight])
    .paddingInner(0)
    .paddingOuter(0)

$: xScale = scaleLinear()
      .domain([0, 80])
      .range([0, innerWidth]);

</script>

<!-- HTML starts here -->
<main>
  
  <h1 style="font-size: 2.5rem; margin-bottom: 1.5rem;">Challenging racial stereotypes at the turn of the 20th century</h1> 
  <hr class='new'>
  <h2 style="font-size: 1.5rem; margin-top: 1rem; margin-bottom: 1rem;">The story of W.E.B. Du Bois and the 1900 Paris Exposition</h2>
  <hr class='new'>
  <br>


  <Narrative/>
  <Quote/>
  <Spacer/>


  <!-- Actual content  -->


  <!-- The image of Du Bois -->
  <section>
    <div class="content">
      <div class='wide-container'>
        <div class="chart" >
            <div class = "image-container">
            <figure> 
              <img 
                alt="Carte-de-visite of Du Bois, with beard and mustache, around 39 years old"
                src='https://github.com/MarkHanly/dubois-challenge-2024/blob/main/src/images/W.E.B._Du_Bois_by_James_E._Purdy,_1907_(cropped).jpg?raw=true'
                width='100%'>
            </figure>
          </div>
          </div>
        </div>

        <div class='overlay'>
          <div class='steps'>
  
            <Scrolly bind:value={currentStep}>
              {#each scene1 as text, i}
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


    <Spacer/>
    <p>Let's take a look at one of the charts presnted by Du Bois, at the Paris Exposition in 1900.</p>
    <Spacer/>
  <!-- This is the chart -->
  
  <section> 
    <h1>Illiteracy of African American people compared with that of other nations</h1>  
    <hr class='new'>
    <h2>Propotion d'illettrés parmi les Afro-Américain comparée à celle des autres nations</h2>
    <hr class='new'>
    <h3>Done by Atlanta University.</h3>
    <div class='content'>
      <div class='wide-container'>
        <div class='chart'>
          <div class='chart-container' bind:clientWidth={width}>
            <svg {width} {height}>
              <g transform="translate({margin.left} {margin.top})">
                {#if currentChartStep <5}
                <Yaxis {yScale} height={innerHeight} {hoveredData}/>
                {/if}
                <!-- Seems to need this block to force refresh the Y-axis? -->
                {#if currentChartStep >= 5} 
                  <Yaxis {yScale} height={innerHeight} {hoveredData}/>
                {/if}
                {#each renderedData as d, i} 
                  <rect
                    x=0
                    y={yScale(d.nation)}
                    width={xScale(d.score)}
                    height=25
                    opacity={hoveredData ? (hoveredData === d ? 1 : 0.6) : 1}
                    fill={d.nation === "Afro-Américain" ? '#be0022' : '#006e41'}
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
              {#each scene3 as text, i}
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


  <Spacer/>
  <h1>Now let's check out the original</h1>  

<!-- The original chart -->
<section>
  <div class='content'>
    
    <div class='wide-container'>
      <div class='chart'>
        <div class = "image-container">
        <figure>
          <img 
            width=100%,  
            alt="The original W.E.B Du Bois chart titled 'Illiteracy of the American Negroes compared with that of other nations.' The chart shows illiteracy rates for 10 population groups, including African Americans of 57%, Romanians (73%) and Swedes (less than 1%)."
            src='https://tile.loc.gov/storage-services/service/pnp/ppmsca/33900/33909r.jpg'>
          <figcaption></figcaption>
        </figure>
      </div>
      </div>
    </div>

    <div class='overlay'>
      <div class='steps'>

        <Scrolly bind:value={currentStep}>
          {#each scene4 as text, i}
          <div class='step' class:active={currentStep===i}>
            <div class='step-content'>
              <p>{@html text}</p>
            </div>
          </div>
          {/each}
        </Scrolly>  
        
      </div>
    </div>
</div>
</section> 

<!-- <div class = 'reference-step'>
  current step: {currentChartStep}
</div> -->


<Denouement/>

</main>
<Footer/>

<!-- CSS styling from here -->

<style>

:global(.tick text) {
    fill: #695a4d;
    user-select: none;
    transition: 
      font-size 300ms ease, 
      opacity 300ms ease,
      y 3s ease;
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
    color: #695a4d;
    max-width: 868px;
    margin: auto;
    font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif
  }

  .chart-container {
    background-image: url("https://raw.githubusercontent.com/Giammaria/Du-Bois-DVS-challenge/main/2024/challenge-07/_artifacts/background.jpg");
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
    max-width: 500px;
    max-height: 800px;
    box-shadow: 1px 1px 30px #695a4d;
    margin-top: 20px;
  }

  .image-container {
    max-width: 500px;
    box-shadow: 1px 1px 30px #695a4d;
    margin-top: 20px;
  }

  h1 {
    padding-top: 0.5rem;
    padding-bottom: 1rem;
    font-size: 1.4em;
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
    border: 0.2px solid #695a4d;
    width:20%;
    opacity: 0.3;
    margin: 0 auto;
  }

  .step {
    height: 90vh;
    opacity: 0.3;
    color: #695a4d;
    transition: opacity 600ms ease;
    display: flex;
    justify-content: center;
    place-items: center;
  }

  .step.active {
    opacity: 1;
  }

  .step-content {
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
    padding: 3px;
    margin-top: 1rem;
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
    /* margin-top: 20px; */
  }

  /* This is the container for the text elements */
  .overlay {
    flex: 1;
    position: relative;
    opacity: 0.8;
    text-align: center;
    padding: 5px;
    z-index: 2;
    justify-content: center;
    place-items: center;
    pointer-events: none;
    max-width: 400px;
    margin: auto;
  }

  .steps {
    flex: 1;
    padding: 20px;
    margin: auto;
    position: relative;
    z-index: 2;
    pointer-events: none;
  }


  /* Rules for wider screens */
  @media (max-width: 700px) {

    main {
    position: relative;
    background-color: #d8cec2;
    padding-left: 1px;
    padding-right: 1px;
    font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif
  }

    .content {
      flex-direction: column;
    }

    .step-content {
    background-color: #d8cec2;
    opacity: 1;
    border: 1px solid #695a4d;
    padding: 0.75rem 1rem;
    border-radius: 8px;
    pointer-events: none;
  }

  .chart-container {
    background-image: url("https://raw.githubusercontent.com/Giammaria/Du-Bois-DVS-challenge/main/2024/challenge-07/_artifacts/background.jpg");
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
    max-width: 900px;
    max-height: 700px;
    box-shadow: 1px 1px 30px #695a4d;
    margin-top: 20px;
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
