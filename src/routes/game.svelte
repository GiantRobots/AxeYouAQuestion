<script lang="ts">
	import Target from '../components/Target.svelte';
	import ScoreBoard from '../components/Scoreboard.svelte';
	import { Writable, writable, derived } from 'svelte/store';

	let player1Name = 'Player 1';
	let player2Name = 'Player 2';

	//
	let player1Score: Writable<number[]> = writable([]);
	let player2Score: number[] = [];

	let numberOfRounds = 5;
	$: rounds = new Array(numberOfRounds);

	let forceReRender = 0;

	//
	let currentScore = derived(player1Score, (player1Score) =>
		player1Score.reduce((previous, current) => (current += previous), 0)
	);

	function formSubmit(form: any) {
		let formData = new FormData(form.srcElement) as any;

		// For some reason formData isn't an iterable and we can't for of this so says typescript
		for (let field of formData) {
			const [key, value] = field;
			// This is weird/lame and we should have a better less hardcoded way to implement this state
			if (key === '1') {
				player1Score.update((score) => score.concat(Number(value)));
			}
			if (key === '2') {
				player2Score.push(Number(value));
			}
		}

		// Reset target selections
		forceReRender = Date.now();
	}
	function SumScore(score: number[]) {
		return score.reduce((previous, current) => (current += previous), 0);
	}
</script>

<svelte:head>
	<title>Let Me Axe you a question</title>
</svelte:head>

<main>
	<h1>Let me Axe you some questions</h1>

	<div class="settings">
		<label>
			Player 1:
			<input bind:value={player1Name} />
		</label>
		<label>
			Player 2:
			<input bind:value={player2Name} />
		</label>
		<label
			><span>Number of rounds</span>
			<input name="numberOfRounds" bind:value={numberOfRounds} type="number" /></label
		>
	</div>

	{#if rounds.length === $player1Score.length && rounds.length === player2Score.length}
		{SumScore($player1Score) > SumScore(player2Score) ? `${player1Name} Wins!` : ''}
		{SumScore(player2Score) > SumScore($player1Score) ? `${player2Name} Wins!` : ''}
		{SumScore($player1Score)}
		{SumScore(player2Score)}
		{SumScore($player1Score) === SumScore(player2Score) ? `It's a draw fools` : ''}
		<ScoreBoard player1Score={$player1Score} {player2Score} {rounds} />
	{:else}
		<form on:submit|preventDefault={formSubmit}>
			<div>
				<button type="submit">Next Round</button>
			</div>
			<div class="flex justify-between">
				{#key numberOfRounds}
					{#key forceReRender}
						<div>
							<Target playerName={player1Name} hiddenFieldName="1" />
						</div>

						<div class="order-first sm:order-none">
							<ScoreBoard player1Score={$player1Score} {player2Score} {rounds} />
						</div>

						<div>
							<Target playerName={player2Name} hiddenFieldName="2" />
						</div>
					{/key}
				{/key}
			</div>
		</form>
	{/if}
</main>

<style>
	label {
		display: block;
	}

	input,
	button {
		font-family: inherit;
		font-size: inherit;
		-webkit-padding: 0.4em 0;
		padding: 0.4em;
		margin: 0 0 0.5em 0;
		box-sizing: border-box;
		border: 1px solid #ccc;
		border-radius: 2px;
	}

	input:disabled {
		color: #ccc;
	}

	button {
		color: #333;
		background-color: #f4f4f4;
		outline: none;
	}

	button:disabled {
		color: #999;
	}

	button:not(:disabled):active {
		background-color: #ddd;
	}

	button:focus {
		border-color: #666;
	}
	/* Our CSS not the global stuff */
	main {
		text-align: center;
		padding: 1em;
		/* min-width: 400px; */
		margin: 0 auto;
	}
	/* .targetStuff {
		display: grid;
		grid-gap: 10px;
		grid-template-columns: repeat(auto-fill, minmax(30vw, auto));
	} */

	/* .targetStuff > * {
		background-color: green;
		/* height: 200px; 
	} */

	/* .targetStuff {
		display: flex;
	}
	.targetStuff > div {
		flex: 1 1 auto;
	} */
	/* h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	} */
</style>
