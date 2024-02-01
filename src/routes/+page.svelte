<article class="text-center grid items-start max-w-max mx-auto max-sm:text-sm">
	<aside class="freeze left-0 z-[1]">
		<h1 class="freeze top-0 left-0">
			<strong>{String(year).slice(2, 4)}</strong>
		</h1>

		{#each weekdays as day}
			{@const d = String(day)[0]}
			<div class:text-rose-600={d.startsWith('S')}>
				{d}
			</div>
		{/each}
	</aside>

	{#each Array(12) as _, m}
		{@const daysInMonth = new Date(year, m + 1, 0).getDate()}
		{@const offset = weekdays.findIndex(
			(d) => d === calendar(m, 1).get('weekday'),
		)}

		<dl>
			{#each Array(offset) as _}
				<div />
			{/each}

			<dt class="freeze top-0">
				{calendar(m).get('month')}
			</dt>

			{#each Array(daysInMonth) as _, d}
				{@const day = calendar(m, d + 1).get('weekday')}
				<dd
					class="tabular-nums"
					class:text-rose-600={String(day)[0] === 'S'}
					class:today={m === now.getMonth() && d === now.getDate() - 1}
				>
					{String(d + 1).padStart(2, '0')}
				</dd>
			{/each}
		</dl>
	{/each}
</article>

<style>
	article {
		grid-template-columns: repeat(13, minmax(max-content, 1fr));
		grid-auto-columns: 1fr;
	}

	aside,
	dl {
		display: grid;
		grid-auto-rows: 1fr;
	}

	.freeze {
		@apply sticky bg-white dark:bg-black;
	}

	.today {
		@apply font-bold dark:underline;
	}
</style>

<script lang="ts">
	const now = new Date()
	const year = now.getFullYear()

	const formatter = new Intl.DateTimeFormat('en', {
		month: 'short',
		weekday: 'long',
	})

	const calendar = (month: number, day: number = 1) =>
		new Map(
			formatter
				.formatToParts(new Date(year, month, day))
				.map(({ type, value }) => [type, value]),
		)

	const weekdays = Array(new Date(year, 0, 0).getDate() + 6)
		.fill(0)
		.map((_, i) => calendar(0, i + 1).get('weekday'))
</script>
