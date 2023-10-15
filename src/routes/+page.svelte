<script>
	let items = [
		{
			question: 'answer',
			answer: 'Antwort, die'
		},
		{
			question: 'question',
			answer: 'Frage, die'
		},
		{
			question: 'first name',
			answer: 'Vorname, der'
		},
		{
			question: 'lastname',
			answer: 'Nachname, der'
		},
		{
			question: 'I would like to pay by card',
			answer: 'Mit Karte bitte'
		}
	];
	let revealed = false;
	let currentItem = 0;
	$: item = items[currentItem];

	let stats = { right: 0, wrong: 0 };

	function handleRight() {
		stats = {
			...stats,
			right: stats.right + 1
		};
		currentItem += 1;
		revealed = false;
	}

	function handleWrong() {
		stats = {
			...stats,
			wrong: stats.wrong + 1
		};
		currentItem += 1;
		revealed = false;
	}

	function handleReveal() {
		if (!revealed) {
			revealed = true;
		}
	}

	let cardProps = {};
	$: if (revealed) {
		cardProps = {};
	} else {
		cardProps = {
			role: 'button'
		};
	}

	$: from = Math.floor(currentItem / items.length) * 100;

	/**
	 * @param {KeyboardEvent} event
	 */
	function handleWindowKeydown(event) {
		if (!revealed) {
			if (event.key === 'Enter') {
				handleReveal();
			}
			return;
		}
		if (!event.altKey) {
			return;
		}

		if (event.key === '1') {
			event.preventDefault();
			handleRight();
			return;
		}

		if (event.key === '2') {
			event.preventDefault();
			handleWrong();
			return;
		}
	}
</script>

<svelte:window on:keydown={handleWindowKeydown} />

<div
	class={`h-2 bg-gradient-to-r from-slate-400 to-transparent  transition-[background-image] dark:from-slate-700`}
	style:--tw-gradient-from-position={`${Math.floor(((currentItem - 1) / items.length) * 100)}%`}
	style:--tw-gradient-to-position={`${Math.ceil((currentItem / items.length) * 100)}%`}
></div>

<div class="grid grow grid-rows-[auto_min-content] gap-8 p-5">
	<div
		class="grid items-center rounded-2xl border-2 border-slate-200 bg-slate-50 pt-2 text-center shadow-sm dark:border-slate-400 dark:bg-slate-700"
		style:grid-template-columns="1fr max-content 1fr"
		style:grid-template-rows="auto 1fr auto 1fr"
		style:grid-template-areas={`
            '. title progress' 
            'question question question' 
            'spacer spacer spacer' 
            'answer answer answer'
        `}
		on:click={handleReveal}
		on:keypress={(event) => {
			if (event.key === 'Enter') {
				handleReveal();
			}
		}}
		role="button"
		tabindex={0}
	>
		<section class="font-bold" style:grid-area="title">DaF Kompakt Lektion 1 / 1</section>
		<section style:grid-area="progress">
			{currentItem + 1} / {items.length + 1}
		</section>
		<section style:grid-area="question" class=" border-bottom text-2xl leading-loose lg:text-6xl">
			{item.question}
		</section>
		<hr class="mx-12" style:grid-area="spacer" />
		<section style:grid-area="answer" class=" text-2xl leading-loose">
			{#if revealed}
				{item.answer}
			{/if}
		</section>
	</div>

	<div>
		{#if revealed}
			<div class="flex gap-3">
				<button
					on:click={handleWrong}
					class="w-full rounded-2xl border-2 border-red-400 p-2 text-xl shadow-sm dark:border-red-500"
					>Nicht gewusst
				<span class="hidden text-sm text-slate-400 lg:inline">[Alt + 2]</span>
				</button>
				<button
					on:click={handleRight}
					class="w-full rounded-2xl border-2 border-green-400 p-2 text-xl shadow-sm dark:border-green-500"
					>Gewusst
					<span class="hidden text-sm text-slate-400 md:inline">[Alt + 1]</span></button
				>
			</div>
		{:else}
			<button
				on:click={() => (revealed = true)}
				class="w-full rounded-2xl border-2 border-slate-200 p-2 text-xl shadow-sm dark:border-slate-400"
				>Aufdecken
				<span class="hidden text-sm text-slate-400 md:inline">[Enter]</span>
			</button>
		{/if}
	</div>
</div>
