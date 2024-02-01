<article class="text-center grid items-start">
	<aside class="grid [grid-auto-rows:1fr]">
		<h1>
			<strong>{year}</strong>
		</h1>

		{#each weekdays as day}
			<div>{day}</div>
		{/each}
	</aside>

	{#each Array(12) as _, m}
		{@const daysInMonth = new Date(year, m + 1, 0).getDate()}
		{@const offset = weekdays.findIndex(
			(d) => d === calendar(m, 1).get('weekday'),
		)}

		<dl class="grid [grid-auto-rows:1fr]">
			{#each Array(offset) as _}
				<div />
			{/each}

			<dt>{calendar(m).get('month')}</dt>
			{#each Array(daysInMonth) as _, d}
				<dd class="tabular-nums">
					{String(d + 1).padStart(2, '0')}
				</dd>
			{/each}
		</dl>
	{/each}
</article>

<style>
	article {
		grid-template-columns: repeat(13, 1fr);
	}
</style>

<script lang="ts">
	const now = new Date()
	const year = now.getFullYear()

	const formatter = new Intl.DateTimeFormat('en', {
		month: 'short',
		weekday: 'short',
	})

	const calendar = (month: number, day: number = 1) =>
		new Map(
			formatter
				.formatToParts(new Date(year, month, day))
				.map(({ type, value }) => [type, value]),
		)

	const weekdays = Array(new Date(year, 0, 0).getDate() + 7)
		.fill(0)
		.map((_, i) => calendar(0, i + 1).get('weekday'))
</script>
