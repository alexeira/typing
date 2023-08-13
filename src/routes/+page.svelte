<script lang="ts">
	type Game = 'waiting for input' | 'in progress' | 'over'
	type Word = string

	let game: Game = 'waiting for input'
	let typedLetter = ''

	let words: Word[] =
		'The quick brown fox jumps over the lazy dog The quick brown fox jumps over the lazy dog The quick brown fox jumps over the lazy dog The quick brown fox jumps over the lazy dog The quick brown fox jumps over the lazy dog'.split(
			' '
		)

	let wordIndex = 0
	let letterIndex = 0
	let correctLetters = 0

	let wordsEl: HTMLDivElement
	let letterEl: HTMLSpanElement
	let inputEl: HTMLInputElement

	function updateGameState() {
		setLetter()
		checkLetter()
		nextLetter()
		updateLine()
		resetLetter()
	}

	function setLetter() {
		const isWordCompleted = letterIndex > words[wordIndex].length - 1

		if (!isWordCompleted) {
			letterEl = wordsEl.children[wordIndex].children[letterIndex] as HTMLSpanElement
		}
	}

	function checkLetter() {
		const currentLetter = words[wordIndex][letterIndex]

		if (typedLetter === currentLetter) {
			letterEl.dataset.letter = 'correct'
			increseScore()
		}

		if (typedLetter !== currentLetter) {
			letterEl.dataset.letter = 'incorrect'
		}
	}

	function increseScore() {
		correctLetters += 1
	}

	function nextLetter() {
		letterIndex += 1
	}

	function nextWord() {
		const isNotFirstLetter = letterIndex !== 0
		const isOneLetterWord = words[wordIndex].length === 1

		if (isNotFirstLetter || isOneLetterWord) {
			wordIndex += 1
			letterIndex = 0
			increseScore()
		}
	}

	function updateLine() {
		const wordEl = wordsEl.children[wordIndex]
		const wordsY = wordsEl.getBoundingClientRect().y
		const wordY = wordEl.getBoundingClientRect().y

		console.log(wordY, wordsY)

		if (wordY > wordsY) {
			wordsEl.scrollIntoView({ block: 'center' })
		}
	}

	function resetLetter() {
		typedLetter = ''
	}

	function startGame() {
		setGameState('in progress')
	}

	function setGameState(state: Game) {
		game = state
	}

	function handleKeydown(event: KeyboardEvent) {
		if (event.code === 'Space') {
			event.preventDefault()

			if (game === 'in progress') {
				nextWord()
			}
		}

		if (game === 'waiting for input') {
			startGame()
		}
	}
</script>

<div class="game" data-game={game}>
	<input
		bind:this={inputEl}
		bind:value={typedLetter}
		on:input={updateGameState}
		on:keydown={handleKeydown}
		class="input"
		type="text"
	/>

	<div bind:this={wordsEl} class="words">
		{#each words as word}
			<span class="word">
				{#each word as letter}
					<span class="letter">{letter}</span>
				{/each}
			</span>
		{/each}
	</div>
</div>

<style>
	.words {
		--line-height: 1em;
		--lines: 3;

		width: 100%;
		max-height: calc(var(--line-height) * var(--lines) * 1.42);
		display: flex;
		flex-wrap: wrap;
		gap: 0.6em;
		position: relative;
		font-size: 1.5rem;
		line-height: var(--line-height);
		overflow: hidden;
		user-select: none;
	}

	.letter {
		opacity: 0.4;
		transition: all 150ms ease;

		&[data-letter='correct'] {
			opacity: 0.8;
		}

		&[data-letter='incorrect'] {
			opacity: 1;
			color: #e14949;
		}
	}
</style>
