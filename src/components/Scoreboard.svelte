<script type="ts">
	export let rounds = new Array(5);

	export let player1Score = new Array(5);
	export let player2Score = new Array(5);
</script>

<table class="scoreBoard">
	<tbody>
		{#each rounds as round, i}
			<tr style="border-bottom: 1px solid dimgray; color: dimgray">
				<td style="border-right: solid 1px dimgray;">
					{#if player1Score[i]}
						{player1Score[i]}
					{:else if player1Score[i] === 0}
						~
					{:else}
						{@html player1Score[i] ?? '&nbsp;'}
					{/if}
				</td>
				<td>
					{#if player2Score[i]}
						{player2Score[i]}
					{:else if player2Score[i] === 0}
						~
					{:else}
						{@html player2Score[i] ?? '&nbsp;'}
					{/if}
				</td>
			</tr>
		{/each}
		<tr style="border-top: solid 2px black; font-size: 36px;">
			<td style="border-right: solid 2px black; ">
				<strong class="playerScore"
					>{player1Score.reduce((previous, current) => (current += previous), 0)}</strong
				>
			</td>
			<td>
				<strong class="playerScore"
					>{player2Score.reduce((previous, current) => (current += previous), 0)}</strong
				>
			</td>
		</tr>
	</tbody>
</table>

<style>
	.scoreBoard {
		border-collapse: collapse;
		width: 100%;
		font-size: 24px;
		margin: 60px auto;
	}
	.scoreBoard tr {
		width: 100%;
	}
	.scoreBoard tr td {
		padding-top: 20px;
	}

	.playerScore {
		font-size: 40px;
		font-weight: 600;
	}
</style>
