<script>
	let revealed = false;
	let item = {
		question: 'answer',
		answer: 'Antwort, die'
	};
</script>

<div class="grid grow grid-rows-[auto_min-content] gap-8">
	<div
		class="flex flex-col justify-between rounded-2xl border-2 border-slate-200 bg-slate-50 p-12 text-center text-2xl leading-loose shadow-sm dark:border-slate-400 dark:bg-slate-700"
	>
		<div class="flex grow flex-col justify-center">{item.question}</div>
		<hr />
		<div class="flex grow flex-col justify-center">
			{#if revealed}
				{item.answer}
			{/if}
		</div>
	</div>

	<div>
		{#if revealed}
			<div class="flex gap-3">
				<button
					class="w-full rounded-2xl border-2 border-red-400 p-2 text-xl shadow-sm dark:border-red-500"
					>Nicht gewusst</button
				>
				<button
					class="w-full rounded-2xl border-2 border-green-400 p-2 text-xl shadow-sm dark:border-green-500"
					>Gewusst</button
				>
			</div>
		{:else}
			<button
				on:click={() => (revealed = true)}
				class="w-full rounded-2xl border-2 border-slate-200 p-2 text-xl shadow-sm dark:border-slate-400"
				>Aufdecken</button
			>
		{/if}
	</div>
</div>
