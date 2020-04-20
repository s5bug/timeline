<script>
	import Event from './Event.svelte';

	function daysIntoYear(date) {
		return (Date.UTC(date.getFullYear(), date.getMonth(), date.getDate()) - Date.UTC(date.getFullYear(), 0, 0)) / 24 / 60 / 60 / 1000;
	}

	const width = 200;

	const events = {
		"1607": {
			"14 May": "Jamestown founded"
		},
		"1619": {
			"30 Jul": "House of Burgesses established"
		},
		"1620": {
			"18 Dec": "Plymouth Colony established"
		},
		"1629": {
			"4 Mar": "Massachusetts Bay Colony founded"
		},
		"1636": {
			"": "Rhode Island Colony founded"
		},
		"1637": {
			"7 Nov": "Anne Hutchinson banished from Massachusetts"
		},
		"1676": {
			"30 Jul": "Start of Bacon's rebellion",
			"19 Sep": "Jamestown burned by Bacon's forces",
			"26 Oct": "Nathaniel Bacon dies of dysentery: End of Bacon's rebellion"
		},
		"1733": {
			"17 May": "Molasses Act passed",
			"24 Jun": "Molasses Act put into partial enforcement",
			"25 Dec": "Molasses Act put into full enforcement"
		},
		"1776": {
			"10 Jan": "Publication of Common Sense by Thomas Paine",
			"2 Jul": "Independence formally declared by the United States",
			"4 Jul": "Final version of the Declaration of Independence signed by Congress",
			"2 Aug": "Final version of the Declaration of Independence fully signed"
		},
		"2012": {
			"6 Nov": "Barrack Obama re-elected for second term as President"
		}
	};

	function yearDateToNum(y, d) {
		const date = new Date(Date.parse(y + " " + d));
		return Number.parseInt(y) + (daysIntoYear(date) / 366);
	}

	const years = Object.keys(events);
	console.log(years)

	const fullEvents = {};
	years.forEach(a => {
		const year = a;
		const yearEvents = events[a];
		const dates = Object.keys(yearEvents);
		return dates.forEach(d => {
			fullEvents[yearDateToNum(year, d)] = {description: yearEvents[d], exactDate: d + " " + year};
		});
	});

	const fullYears = Object.keys(fullEvents);

	const firstYear = Math.min(...fullYears.map(Number.parseFloat));
	const lastYear = Math.max(...fullYears.map(Number.parseFloat));

	const fBefore = Math.floor(firstYear);
	const fAfter = Math.ceil(lastYear);

	for(var i = fBefore; i < fAfter; i++) {
		fullEvents[i] = fullEvents[i] || {description: "", exactDate: i.toString()}
	}

	const fullEntries = Object.entries(fullEvents);

	const smallestGap = Math.min(...fullYears.slice(0, fullYears.length - 1).map((a, i) => fullYears[i + 1] - a));

	const xPerYear = (width / smallestGap) * 1.2;

	const bottomW = (xPerYear * (lastYear - firstYear)) + width;

	function scrollHorizontally(e) {
		e = window.event || e;
		let delta = Math.max(-1, Math.min(1, (e.wheelDelta || -e.detail)));
		document.getElementsByTagName('html')[0].scrollLeft -= (delta*40); // Multiplied by 40
		e.preventDefault();
	}
</script>

<style>
	.timeline {
		border-bottom: 1px solid black;
		height: calc(100vh - 4em);
	}
</style>

<main id="main">
	<div id="timeline" on:mousewheel={scrollHorizontally} class="timeline" style="width: {bottomW}px;">
		{#each fullEntries as [fullYear, p]}
			<Event x="{xPerYear * (fullYear - firstYear) + 24}" w="{width}" year="{p.exactDate}">
				{p.description}
			</Event>
		{/each}
	</div>
</main>
