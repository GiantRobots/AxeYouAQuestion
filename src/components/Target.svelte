<script lang="ts">
	export let playerName: string;
	export let hiddenFieldName: string;

	export let currentPointSelection:
		| ''
		| 'kipped'
		| 'center'
		| 'second'
		| 'outside'
		| 'clutch-top-left'
		| 'clutch-top-right' = '';

	let currentPoints: number | undefined = undefined;

	export let centerPoints: number = 5;
	export let secondPoints: number = 3;
	export let outsidePoints: number = 1;
	export let clutchPoints: number = 7;
</script>

<div>
	<h2>{playerName}</h2>
	<div class="target-container">
		<div
			class="target clutch top-left"
			class:selected={currentPointSelection === 'clutch-top-left'}
			on:click={() => {
				currentPointSelection = 'clutch-top-left';
				currentPoints = clutchPoints;
			}}
		/>

		<div
			class="target outside"
			class:selected={currentPointSelection === 'outside'}
			on:click={() => {
				currentPointSelection = 'outside';
				currentPoints = outsidePoints;
			}}
		/>
		<div
			class="target second"
			class:selected={currentPointSelection === 'second'}
			on:click={() => {
				currentPointSelection = 'second';
				currentPoints = secondPoints;
			}}
		/>
		<div
			class="target center"
			class:selected={currentPointSelection === 'center'}
			on:click={() => {
				currentPointSelection = 'center';
				currentPoints = centerPoints;
			}}
		/>

		<div
			class="target clutch top-right"
			class:selected={currentPointSelection === 'clutch-top-right'}
			on:click={() => {
				currentPointSelection = 'clutch-top-right';
				currentPoints = clutchPoints;
			}}
		/>
		<button
			type="button"
			class="kipped rounded-md shadow bg-indigo-600"
			class:selected={currentPoints === 0}
			on:click={() => {
				currentPointSelection = 'kipped';
				currentPoints = 0;
			}}>Kipped</button
		>
	</div>
	<input type="hidden" name={hiddenFieldName} bind:value={currentPoints} />
</div>

<style>
	.target-container {
		position: relative;
		text-align: center;
		margin: 0 auto;
		/* Make a square that takes x% of the screen */
		width: 40vw;
		height: 40vw;
	}
	.target {
		border-radius: 100%;
		position: absolute;
		border: 5px solid black;
		height: 5px;
		width: 5px;
		background: white;
		/* Get everything center aligned */
		top: 50%;
		left: 50%;
		margin-right: -50%;
		transform: translate(-50%, -50%);
	}
	.center {
		width: 25%;
		height: 25%;
	}
	.center:hover,
	.center.selected {
		background: #999;
	}
	.second {
		width: 65%;
		height: 65%;
		border-color: red;
	}
	.second:hover,
	.second.selected {
		background-color: #f9acac;
	}
	.outside {
		width: 90%;
		height: 90%;
		border-color: blue;
	}
	.outside:hover,
	.outside.selected {
		background-color: #0000ff85;
	}

	.clutch {
		width: 10%;
		height: 10%;
		border-color: green;
		/* Unset the og target declarations */
		transform: none;
		margin-right: unset;
		left: unset;
	}
	.clutch:hover,
	.clutch.selected {
		background: #00800085;
	}
	.clutch.top-left {
		top: 0px;
		left: 0px;
	}
	.clutch.top-right {
		top: 0px;
		right: 0px;
	}

	.kipped {
		position: absolute;
		/* border: 4px #65727a solid;
		border-radius: 10px 10px 0 0;
		background: #8f9a9c;
		color: white;
		font-weight: 500;
		font-size: 24px; */

		bottom: -100px;

		/* Center align horizontally  */
		left: 50%;
		margin-right: -50%;
		transform: translate(-50%, -50%);
	}
	.kipped.selected,
	.kipped:active {
		background: #65727a;
	}
</style>
