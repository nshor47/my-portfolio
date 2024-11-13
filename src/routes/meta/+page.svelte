<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';

    let data = [];
    let commits = [];
    let numFiles, avgFileLength, avgLineLength, maxWorkDay, maxPeriod;
    let xScale, yScale;
    let xAxis, yAxis, yAxisGridlines;



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

$: {
    d3.select(xAxis).call(d3.axisBottom(xScale)),
    d3.select(yAxis).call(d3.axisLeft(yScale));
    d3.select(yAxis).call(
    d3.axisLeft(yScale).tickFormat((d) => String(d % 24).padStart(2, '0') + ':00'),
    d3.select(yAxisGridlines).call(
        d3.axisLeft(yScale).tickFormat('').tickSize(-usableArea.width),
    )
);
}

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

<br>
<h2>Commits by Time of Day</h2>
<svg viewBox="0 0 {width} {height}">
    <g transform="translate(0, {usableArea.bottom})" bind:this={xAxis} />
    <g transform="translate({usableArea.left}, 0)" bind:this={yAxis} />
    <g class="gridlines" transform="translate({usableArea.left}, 0)" bind:this={yAxisGridlines} />
    <g class="dots">
        {#each commits as commit, index }
        <circle
          cx="{xScale(commit.datetime)}"
          cy="{yScale(commit.hourFrac)}"
          r="5"
          fill="steelblue"
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
</style>