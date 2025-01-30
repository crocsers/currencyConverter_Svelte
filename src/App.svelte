<script>
	import { onMount } from 'svelte';
  
	let initialAmount = '';
	let convertedAmount = '';
	let fromCurrency = 'PHP';
	let toCurrency = 'USD';
	let rates = {};
  
	const currencies = ['PHP', 'USD', 'EUR', 'GBP', 'JPY'];
  
	async function fetchRates() {
	  const response = await fetch('https://api.exchangerate-api.com/v4/latest/PHP');
	  const data = await response.json();
	  rates = data.rates;
	}
  
	onMount(() => {
	  fetchRates();
	});
  
	function convertCurrency() {
	  if (!isNaN(initialAmount) && initialAmount !== '') {
		const value = parseFloat(initialAmount);
		const fromRate = rates[fromCurrency];
		const toRate = rates[toCurrency];
		const convertedValue = (value / fromRate) * toRate;
		convertedAmount = convertedValue.toFixed(2);
	  } 
	  else {
		convertedAmount = 'Invalid input';
	  }
	}
  
	function clearFields() {
	  initialAmount = '';
	  convertedAmount = '';
	  fromCurrency = 'PHP';
	  toCurrency = 'USD';
	}
  </script>
  
  <main>
	<h1>Currency Converter</h1>
	
	<div class="flex-container">
		<div class="input-group">
			<label for="initialAmount">Amount: </label>
			<input id="initialAmount" type="text" bind:value={initialAmount} 
				placeholder="Please Enter Amount" pattern="^\d*\.?\d*$" /> <!--Only Allow Float-->
			<label for="fromCurrency">Convert From: </label>
			<select id="fromCurrency" bind:value={fromCurrency}>
				{#each currencies as currentCode}
					<option value={currentCode}>{currentCode}</option>
				{/each}
			</select>
		</div>

		<div class="input-group">
			<label for="convertedAmount">Converted Amount: </label>
			<input id="convertedAmount" type="text" bind:value={convertedAmount} 
				placeholder="Please Enter Amount" pattern="^\d*\.?\d*$" /> <!--Only Allow Float-->
			<label for="toCurrency">Convert From: </label>
			<select id="toCurrency" bind:value={toCurrency}>
				{#each currencies as currentCode}
					<option value={currentCode}>{currentCode}</option>
				{/each}
			</select>
		</div>
	</div>

	<div class="button-container">
		<button on:click={convertCurrency}>Convert</button>
		<button on:click={clearFields}>Clear</button>

	</div>
	
	
  </main>
  
  <style>

	:global(body){
		margin: 0;
		height: 100vh;
		background: linear-gradient(135deg, #1E90FF, #87CEEB);
		display: flex;
		justify-content: center;
		align-items: center;
	}
	main {
		max-width: 450px;
	  	margin: auto;
	  	padding: 1em;
	  	border: 1px solid #ccc;
	  	border-radius: 5px;
		background: rgba(255, 255, 255, 0.9);
   		color: black;
  
	}
  
	.flex-container {
		display: flex;
		justify-content: space-between;
		margin: 20px 0;
	}

	.button-container{
		display: flex;
		justify-content: center;
		margin-top: 20px
	}

	.input-group {
		display: flex;
		flex-direction: column;
		margin-right: 25px;
	}

	.input-group label{
		margin-bottom:5px;
	}

	.input-group input, .input-group select{
		padding: 10px;
		margin-bottom: 15px;
	}

	div{
		margin-bottom: 1em;
	}

	button {
		width: 85px;
		margin: 10px;
		padding: 10px;
		border: 2px solid #cccccc;
		border-radius: 5px;
		cursor: pointer;
		background-color: #b2d4f6;
		transition: background-color 0.5s;
	}

	button:hover{
		background-color:#1E90FF;
		border-radius: 5px;
	}
  </style>