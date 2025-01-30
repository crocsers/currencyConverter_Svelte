<script>
	import { onMount } from 'svelte';
  
	let amount = '';
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
	  if (!isNaN(amount) && amount !== '') {
		const value = parseFloat(amount);
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
	  amount = '';
	  convertedAmount = '';
	  fromCurrency = 'PHP';
	  toCurrency = 'USD';
	}
  </script>
  
  <main>
	<h1>Currency Converter</h1>
	
	<div>
	  <label for="amount">Amount:</label>
	  <input id="amount" type="text" bind:value={amount} placeholder="Enter amount" pattern="^\d*\.?\d*$"
	  />
	</div>
  
	<div>
	  <label for="fromCurrency">Convert From:</label>
	  <select id="fromCurrency" bind:value={fromCurrency}>
		{#each currencies as code}
		  <option value={code}>{code}</option>
		{/each}
	  </select>
	</div>
  
	<div>
	  <label for="toCurrency">Convert To:</label>
	  <select id="toCurrency" bind:value={toCurrency}>
		{#each currencies as code}
		  <option value={code}>{code}</option>
		{/each}
	  </select>
	</div>
  
	<div>
	  <button on:click={convertCurrency}>Convert</button>
	  <button on:click={clearFields}>Clear</button>
	</div>
  
	<div>
	  <label for="converted">Converted Amount:</label>
		<input id="converted" type="text" value={convertedAmount} readonly/>
	</div>
  </main>
  
  <style>
	main {
	  max-width: 400px;
	  margin: auto;
	  padding: 1em;
	  border: 1px solid #ccc;
	  border-radius: 5px;
	}
  
	div {
	  margin-bottom: 1em;
	}
  
	label {
	  display: block;
	  margin-bottom: 0.5em;
	}
  
	input, select {
	  width: 100%;
	  padding: 0.5em;
	  font-size: 1em;
	}
  
	button {
	  margin-right: 0.5em;
	}
  </style>