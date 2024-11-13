<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';

    let data = [];
    let commits = [];
    let numFiles, avgFileLength, avgLineLength, maxWorkDay, maxPeriod, maxDepth;


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
    });

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