<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';

    let data = [];
    let commits = [];
    let numFiles, avgFileLength, avgLineLength, maxWorkDay, maxPeriod;
    let xScale, yScale;
    let xAxis, yAxis, yAxisGridlines;
    let cursor = { x: 0, y: 0 };
    let rScale;
    let svg;
    let hoveredCommit = null;

    onMount(async () => {
        data = await d3.csv('loc.csv', (row) => ({
            ...row,
            line: Number(row.line),
            depth: Number(row.depth),
            length: Number(row.length),
            date: new Date(row.date + 'T00:00' + row.timezone),
            datetime: new Date(row.datetime),
        }));
        commits = d3
            .groups(data, (d) => d.commit)
            .map(([commit, lines]) => {
                let first = lines[0];
                let { author, date, time, timezone, datetime } = first;
                let ret = {
                id: commit,
                url: 'https://github.com/vis-society/lab-7/commit/' + commit,
                author,
                date,
                time,
                timezone,
                datetime,
                hourFrac: datetime.getHours() + datetime.getMinutes() / 60,
                totalLines: lines.length,
                };

                Object.defineProperty(ret, 'lines', {
                value: lines,
                configurable: true,
                writable: true,
                enumerable: false,
                });

                return ret;
            });

            
            commits = d3.sort(commits, (d) => -d.totalLines);

            numFiles = d3.groups(data, d => d.file).length;

            const fileLengths = d3.rollups(data, v => d3.max(v, d => d.line), d => d.file);
            avgFileLength = d3.mean(fileLengths, d => d[1]).toFixed(2);

            avgLineLength = d3.mean(data, d => d.length).toFixed(2);

            const workByDay = d3.rollups(data, v => v.length, d => new Date(d.datetime).toLocaleString('en', { weekday: 'long' }));
            maxWorkDay = d3.greatest(workByDay, d => d[1])?.[0];

            const workByPeriod = d3.rollups(data, v => v.length, d => new Date(d.datetime).toLocaleString('en', { dayPeriod: 'short' }));
            maxPeriod = d3.greatest(workByPeriod, d => d[1])?.[0];

            xScale = d3.scaleTime()
            .domain(d3.extent(commits, d => d.datetime))
            .range([usableArea.left, usableArea.right])
            .nice();
            yScale = d3.scaleLinear()
            .domain([0, 24])
            .range([usableArea.bottom, usableArea.top]);

            const totalLinesExtent = d3.extent(commits, d => d.totalLines);
            rScale = d3.scaleSqrt() 
                .domain(totalLinesExtent)
                .range([2, 30]); 
    });
    
let width = 1000,
    height = 600;

let margin = { top: 10, right: 10, bottom: 30, left: 20 };

let usableArea = {
  top: margin.top,
  right: width - margin.right,
  bottom: height - margin.bottom,
  left: margin.left,
};

usableArea.width = usableArea.right - usableArea.left;
usableArea.height = usableArea.bottom - usableArea.top;

xScale = d3.scaleTime()
            .domain(d3.extent(commits, d => d.datetime))
            .range([usableArea.left, usableArea.right])
            .nice();

yScale = d3.scaleLinear()
      .domain([0, 24])
      .range([usableArea.bottom, usableArea.top]);

function brushed(evt) {
  console.log(evt);
}

$: {
    d3.select(xAxis).call(d3.axisBottom(xScale));
    d3.select(yAxis).call(d3.axisLeft(yScale));
    d3.select(yAxis).call(
        d3.axisLeft(yScale).tickFormat((d) => String(d % 24).padStart(2, '0') + ':00')
    );
    d3.select(yAxisGridlines).call(
        d3.axisLeft(yScale).tickFormat('').tickSize(-usableArea.width)
    );
    d3.select(svg).call(d3.brush().on('start brush end', brushed));
    d3.select(svg).selectAll('.dots, .overlay ~ *').raise();
}



let hoveredIndex = -1;

$: hoveredCommit = commits[hoveredIndex] ?? hoveredCommit ?? {};


</script>


<h1>Metadata</h1>
<h2>Summary</h2>
<dl class="stats">
    <dt>Total <abbr title="Lines of code">LOC</abbr></dt><dd>{data.length}</dd>
    
    <dt>Commits</dt><dd>{commits.length}</dd>
    
    <dt>Total Files</dt><dd>{numFiles}</dd>
    
    <dt>Average File Length (Lines)</dt><dd>{avgFileLength}</dd>
    
    <dt>Average Line Length (Characters)</dt><dd>{avgLineLength}</dd>
    
    <dt>Day with Most Work</dt><dd>{maxWorkDay}</dd>
    
    <dt>Time of Day with Most Work</dt><dd>{maxPeriod}</dd>
</dl>

<dl
  id="commit-tooltip"
  class="tooltip"
  hidden={hoveredIndex === -1} 
  style="top: {cursor.y + 300}px; left: {cursor.x + 10}px;">

  <dt>Commit:</dt>
  <dd><a href="{hoveredCommit.url}" target="_blank">{ hoveredCommit.id }</a></dd>

  <dt>Date:</dt><dd>{ hoveredCommit.datetime?.toLocaleString("en", {dateStyle: "full"}) }</dd>

  <dt>Time:</dt><dd>{ hoveredCommit.datetime?.toLocaleString("en", { timeStyle: "short" }) }</dd>

  <dt>Author:</dt><dd>{ hoveredCommit.author }</dd>

  <dt>Lines Edited:</dt><dd>{ hoveredCommit.totalLines }</dd>
</dl>

<br>
<h2>Commits by Time of Day</h2>
<svg bind:this={svg} viewBox="0 0 {width} {height}">
    <g transform="translate(0, {usableArea.bottom})" bind:this={xAxis} />
    <g transform="translate({usableArea.left}, 0)" bind:this={yAxis} />
    <g class="gridlines" transform="translate({usableArea.left}, 0)" bind:this={yAxisGridlines} />
    <g class="dots">
        {#each commits as commit, index}
            <circle
              cx="{xScale(commit.datetime)}"
              cy="{yScale(commit.hourFrac)}"
              r="{rScale(commit.totalLines)}"
              fill="steelblue"
              fill-opacity="0.7"
              on:mouseenter={evt => {
                hoveredIndex = index;
                cursor = { x: evt.x, y: evt.y };
              }}
              on:mouseleave={evt => {
                hoveredIndex = -1;
                cursor = { x: -9999, y: -9999 };
              }}
            />
        {/each}
    </g>
</svg>



<style>
    svg {
      overflow: visible;
    }
    .gridlines {
      stroke-opacity: 0.2;
    }


    .tooltip {
    position: absolute; 
    background-color: rgba(218, 218, 218, 0.7); 
    color: black;
    border-radius: 4px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
    padding: 5px; 
    pointer-events: none; 
    transition: opacity 500ms ease, visibility 500ms ease; 
    }

    .tooltip[hidden] {
    opacity: 0; 
    visibility: hidden; 
    }

    .tooltip a{
    color: black;
    
    }

    circle {
    transition: 200ms;
    transform-origin: center;
    transform-box: fill-box;
    }

    circle:hover {
    transform: scale(1.5);
    }
</style>